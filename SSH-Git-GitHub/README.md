# Complete SSH Setup for GitHub with Ed25519

## Step 1: Check if you already have SSH keys
```bash
ls -la ~/.ssh
```

Look for files named `id_rsa` and `id_rsa.pub` (or `id_ed25519` and `id_ed25519.pub`). If you see these, you already have SSH keys and can skip to Step 3.

```
NOTE: I generally prefer to generate new key for new device/ purpose.
```

## Step 2: Generate a new SSH key
```bash
ssh-keygen -t ed25519 -C "email@email.com"
```

> NOTE: 
> 1. Keep email in `" "` <br>
> 2. Refer to [description.md](./description.md) for a detailed explanation of the command.

<br>


When prompted:
- First, enter the name and location of the file to be saved (or press `ENTER` for the default loc/ name)
- Enter a passphrase (optional)
- Confirm the passphrase

## Step 3: Add SSH key to the ssh-agent

Start the ssh-agent:
```bash
eval "$(ssh-agent -s)"
```

Add your SSH private key (i.e. file without `.pub`)
```bash
ssh-add ~/.ssh/id_ed25519_filename
```

> NOTE:  <br>
> Refer to [description.md](./description.md) for a detailed explanation of the command.

## Step 4: Copy your public key

Copy the content of the public key:
```bash
cat ~/.ssh/id_ed25519.pub
```

This will show something like:
```
ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIG... email@email.com
```

Copy this entire output.

## Step 5: Add the key to GitHub

1. Go to `github.com` and sign in
2. Click your profile picture → Settings
3. In the left sidebar, click "SSH and GPG keys"
4. Click "New SSH key"
5. Give it a title (example: `name-date`)
6. Paste your public key in the "Key" field
7. Click "Add SSH key"

## Step 6: Test your connection

```bash
ssh -T git@github.com
```

You should see a message like:
```
Hi username! You've successfully authenticated, but GitHub does not provide shell access.
```

## Step 7: Clone using SSH

Now when you go to clone a repository, select the SSH option and use:
```bash
git clone git@github.com:username/repository.git
```

That's it! You can now clone, push, and pull without entering your password each time. Your new Ed25519 key will work for all GitHub operations without needing to enter passwords.

## Debugging
While doing `git clone/ git push`, if you get the following error:
```
git@github.com: Permission denied (publickey).
fatal: Could not read from remote repository.

Please make sure you have the correct access rights
and the repository exists.
```

Then do the following: <br>
- `ls -al ~/.ssh` -> Ensure `id_ed25519_name.pub` exists
- If `.pub` file does not exist, generate a new one (follow Step 1-6)
- If it already exists, add the key to the SSH agent (i.e. Step 3):

    ```
    ssh-add ~/.ssh/id_ed25519_name
    ```
    (Here, you have to enter the passphrase used to create the key!)
- Test the connection (i.e. Step 6):

    ```
    ssh -T git@github.com
    ```

## Why Ed25519 is better than RSA

**Ed25519 advantages:**
- **More secure** - Uses modern elliptic curve cryptography that's resistant to quantum attacks
- **Faster** - Much quicker to generate, sign, and verify
- **Smaller** - Key files are tiny (68 characters vs 2048+ for RSA)
- **Future-proof** - The newer standard that's widely adopted
- **Better performance** - Less CPU overhead during SSH operations

<br>

