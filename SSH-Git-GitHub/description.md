
## SSH vs HTTPS Cloning
### Authentication

**SSH:**
- Uses SSH keys (public/private key pairs)
- No username/password needed once set up
- Keys stored locally, public key added to GitHub

**HTTPS:**
- Uses username + password or personal access token
- Must authenticate for each push (unless cached)
- Credentials can be stored in credential manager

### URLs

**SSH:**
```bash
git clone git@github.com:username/repo.git
```

**HTTPS:**
```bash
git clone https://github.com/username/repo.git
```

### Security & Convenience

**SSH:**
- More secure (cryptographic keys vs passwords)
- More convenient once set up (no repeated authentication)
- Keys can have passphrases for extra security
- Harder initial setup

**HTTPS:**
- Easier initial setup (just need GitHub account)
- Works through corporate firewalls better
- Less secure if using passwords
- Can use personal access tokens for better security

### Network & Firewall

**SSH:**
- Uses port 22
- May be blocked by some corporate firewalls
- Direct encrypted connection

**HTTPS:**
- Uses port 443 (standard web traffic)
- Almost never blocked by firewalls
- Works everywhere the web works

### Performance

**SSH:**
- Slightly faster for frequent operations
- Less overhead once connected

**HTTPS:**
- Minimal performance difference in practice
- Slightly more overhead per operation

### Bottom Line

- **SSH**: Better for frequent Git users, more secure, convenient after setup
- **HTTPS**: Better for beginners, works everywhere, easier troubleshooting

Most developers eventually switch to SSH once they're doing regular Git work.

<br>

## Understanding the ssh-keygen command
```bash
ssh-keygen -t ed25519 -C "email@email.com"
```
This command generates a new SSH key pair using the Ed25519 algorithm. Let me break it down:

**ssh-keygen** - The command-line tool for creating SSH keys

**-t ed25519** - Specifies the type of key to create
- Ed25519 is a modern, secure elliptic curve algorithm
- It's faster and more secure than older RSA keys
- Creates smaller key files (more efficient)
- GitHub recommends this over RSA for new keys

**-C "email@email.com"** - Adds a comment to identify the key
- The `-C` flag stands for "comment"
- You should replace this with your actual email address
- This comment appears at the end of your public key
- It helps you identify which key is which when you have multiple keys
- It's also useful for GitHub to associate the key with your account

When you run this command, it creates two files:
- `~/.ssh/id_ed25519_filename` (private key - keep this secret!)
- `~/.ssh/id_ed25519_filename.pub` (public key - this is what you share)

<br>

## Understanding the eval command
```bash
eval "$(ssh-agent -s)"
```
This command starts the SSH agent and configures your current terminal session to use it. Let me break it down:

**ssh-agent -s** 
- Starts the SSH agent daemon in the background
- The `-s` flag tells it to output shell commands (for bash/zsh)
- Without `-s`, it would output csh/tcsh commands instead

**$(...)**
- This is "command substitution" - it runs the command inside and captures its output
- So `$(ssh-agent -s)` runs ssh-agent and captures what it prints

**eval**
- Takes the captured output and executes it as shell commands
- This actually applies the environment variables to your current session

When you run `ssh-agent -s`, it outputs something like:
```
SSH_AUTH_SOCK=/tmp/ssh-agent.12345.socket; export SSH_AUTH_SOCK;
SSH_AGENT_PID=12346; export SSH_AGENT_PID;
echo Agent pid 12346;
```

The `eval` command then executes these lines, which:
- Sets the `SSH_AUTH_SOCK` environment variable (points to the agent's socket)
- Sets the `SSH_AGENT_PID` environment variable (the agent's process ID)
- Prints "Agent pid 12346"

The SSH agent manages your SSH keys in memory so you don't have to enter your passphrase every time. But your terminal needs to know how to talk to this agent - that's what these environment variables do.

## Troubleshooting

If connection fails:
```bash
ssh -vT git@github.com
```

This shows detailed debug information.

## Configure Git (if needed)

Set your Git identity:
```bash
git config --global user.name "Your Name"
git config --global user.email "your_email@example.com"
```