# gpg-setup


Setup GPG Verification & Set Passphrase Cache Limit <br> <br> 


## Setup:
[Article](https://daily-dev-tips.com/posts/how-to-verify-your-commits-on-github/)
<br>
[YouTube](https://www.youtube.com/watch?v=u9L5kDlU8rs&t=907s)

```
NOTE: brew is installed before proceeding; o/w install brew first
```

### Step 1:

```
gpg --full-generate-key
```

- It will bring up a lot of prompts. Select the following:
- (1) RSA and RSA: `1`
- Keysize: `4096`
- Key is valid for: `0 = key does not expire`
- Is this correct? `y`
- Real name, email: `First-name Last-name`, `GitHub Email`
- Comment: `Text input`

<br>

Output of `gpg --list-secret-keys --keyid-format LONG`
```
sec   rsa4096/KEY_ID 2023-10-15 [SC] 
      string_1
uid   [ultimate] Full Name (comment) <email>
ssb   rsa4096/string_2 2023-10-15 [E]
```



### Step 2:
Using `KEY_ID` from step 1, run the following:
```
gpg --armor --export KEY_ID
```

<br>

Output: It will generate a large block of string:
```
-----BEGIN PGP PUBLIC KEY BLOCK-----
              STRING
-----END PGP PUBLIC KEY BLOCK-----
```

### Step 3:
- Go to github.com
- Settings > SSH and GPG Key > New GPG Key
- Copy/paste everything from Step 2 (including comments)
- Save the key


### Step 4: 
Now, we want to use this key to sign our commits. To do so, run the following commands:
```
git config --global user.signingkey KEY_ID
git config --global commit.gpgsign true
```


<br>

## Potential Issues
### 1. TimeLimit:
[Superuser Discussion](https://superuser.com/questions/624343/keep-gnupg-credentials-cached-for-entire-user-session)

#### Steps:
- Finder > Go > Go To Folder (Or, CMD + SHFT + G)
- Type `~/.gnupg/`
- Navigate to -> `gpg-agent.conf`
- Change Time (in seconds)
- `gpgconf --kill gpg-agent`  
<br>

### 2. Can't Sign The Data:
If: 
>   error: gpg failed to sign the data <br> 
>   fatal: failed to write commit object

Then: <br> 
- `gpgconf --kill all`
- `gpg-agent --daemon`

#### Step: <br> 
- Run: `brew install pinentry-mac`
- CMD + SHIFT + G
- Type `~/.gnupg/`
- Navigate to -> `gpg-agent.conf`
- Add `pinentry-program /opt/homebrew/bin/pinentry-mac`
- Navigate to -> `gpg.conf`
- Add `use-agent`
[More Here](https://gist.github.com/phortuin/cf24b1cca3258720c71ad42977e1ba57) <br> <br>

## Debugging: <br>

`echo "test"| gpg --clearsign` 

If: 
> Inappropriate ioctl for device

Then: <br>
<br> 
  Add `export GPG_TTY=$(tty)` to:
  - either `~/.bash_profile` or
  - `~/.zshrc`

<br>

```
NOTE: Copy/paste content or look up the content of the zshrc file for easy debuggging.
```


## Resources
[Stackoverflow](https://stackoverflow.com/questions/41052538/git-error-gpg-failed-to-sign-data) <br>
[GitHub](https://gist.github.com/paolocarrasco/18ca8fe6e63490ae1be23e84a7039374)
