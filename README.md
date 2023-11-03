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

If: 
>   error: gpg failed to sign the data <br> 
>   fatal: failed to write commit object

Then: <br> 
- `gpgconf --kill all`
- `gpg-agent --daemon`

<br>
<br> 

## Debugging: <br>

`echo "test"| gpg --clearsign` 

If: 
> Inappropriate ioctl for device

Then: <br>
<br> 
  Add `export GPG_TTY=$(tty)` to the `~/.bash_profile` file.
