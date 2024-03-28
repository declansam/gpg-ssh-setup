# gpg-setup


Setup GPG Verification & Set Passphrase Cache Limit <br> <br> 


## Setup:
[Article](https://daily-dev-tips.com/posts/how-to-verify-your-commits-on-github/)
<br>
[YouTube](https://www.youtube.com/watch?v=u9L5kDlU8rs&t=907s)



<br>

## TimeLimit:
[Superuser Discussion](https://superuser.com/questions/624343/keep-gnupg-credentials-cached-for-entire-user-session) <br> <br>

### Steps: <br>
- Finder > Go > Go To Folder (Or, CMD + SHFT + G)
- Type `~/.gnupg/`
- Navigate to -> `gpg-agent.conf`
- Change Time (in seconds)
- `gpgconf --kill gpg-agent`  
<br> <br>

## Can't Sign The Data: <br>
If: 
>   error: gpg failed to sign the data <br> 
>   fatal: failed to write commit object

Then: <br> 
- `gpgconf --kill all`
- `gpg-agent --daemon`

<br>

### Step: <br> 
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
  Add `export GPG_TTY=$(tty)` to the `~/.bash_profile` file.

<br>

[Stackoverflow](https://stackoverflow.com/questions/41052538/git-error-gpg-failed-to-sign-data)
[GitHub](https://gist.github.com/paolocarrasco/18ca8fe6e63490ae1be23e84a7039374)
