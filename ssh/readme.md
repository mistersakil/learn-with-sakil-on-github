# What is SSH and Why You Need It

SSH (Secure Shell) via OpenSSH is a protocol for-

* Secure remote login
* Secure file transfer
* Cryptographic authentication (no passwords)

## Why SSH for GitHub (instead of HTTPS)

***HTTPS:*** Requires username + token (manual or stored)
***SSH:*** Uses key-based authentication (automated, secure)

## How to Check Supported Key Types (Client Side)

***List supported signature algorithms:*** `ssh -Q key-sig | ssh -Q key-sig`
***List supported public key authentication methods:*** `ssh -Q PubkeyAcceptedAlgorithms`
***Check supported host key algorithms:*** `ssh -Q HostKeyAlgorithms`

## Check What Your Server Accepts

***Inspect SSH daemon config:*** `cat /etc/ssh/sshd_config`
***Check OpenSSH Version (Important):*** `ssh -v`

## How to generate key

|Command|Meaning|
|-------|-------|
|ssh-keygen|generate key pair|
|-t ed25519|choose algorithm / type of key|
|-f ~/.ssh/github_ed25519|custom key name/file/location|
|ssh|connect to server|
|-i|specify private key|
|user@host|login target|

***Generate SSH Key (on server):*** `ssh-keygen -t ed25519 -f ~/.ssh/github_ed25519 -C "deploy@your-server"`

### Breakdown

`ssh-keygen`

* CLI utility to generate SSH key pairs
* Outputs:
  * Private key (secret)
  * Public key (shared)

`-t ed25519`

* -t = type
* ed25519 = modern elliptic curve algorithm

`-f ~/.ssh/github_ed25519`

* -f = file path (output location + filename)
* Instead of default: ~/.ssh/id_ed25519

#### Why use custom name?

* Manage multiple keys (e.g., GitHub, production server, staging)
* Avoid overwriting default keys

## What happens after running

***Passphrase:*** `Enter passphrase (empty for no passphrase):`

* Encrypts private key locally
* Adds extra security layer

## Resulting files

|Command|Meaning|
|-------|-------|
|github_ed25519|Private key (never share)|
|github_ed25519.pub|public key (copy to server/GitHub)|

## Add SSH Key to GitHub

***Linux/UNIX:*** cat ~/.ssh/github_ed25519.pub

* Go to Settings > SSH and GPG keys
* Click "New SSH key"
* Add title and Paste your public key
* Click "Add SSH key"

## Test SSH Connection

***Open terminal and write:*** `ssh -i ~/.ssh/github_ed25519 git@github.com`

***Breakdown:***

`ssh` : SSH client used to connect to remote machine

`-i ~/.ssh/github_ed25519`:

* -i = identity file (private key to use)
* This overrides default behavior:
  * Normally SSH tries:
    * ~/.ssh/id_rsa
    * ~/.ssh/id_ed25519
  * Here you force it to use:
    * github_ed25519

`git@github.com`

* user - Remote system username, Example: ubuntu, root, deploy

* host - Server address: IP → 192.168.1.10 , Domain → example.com

## Pull github public repo using Use SSH URLs

***Clone with SSH:*** `git clone git@github.com:username/repository.git`

```PullNode
- Be ensure you already installed git cli on your server. 
- yum install git -y || dnf install git -y(CentOS / AlmaLinux / RHEL).
- Check version: git --version
```
