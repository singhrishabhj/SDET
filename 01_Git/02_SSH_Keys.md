# SSH Keys & GitHub Authentication

## What are SSH Keys?

When we connect our local computer to GitHub, GitHub needs a way to verify that the computer is authorized to access the account/repository.

One way to authenticate with GitHub is using **SSH keys**.

SSH stands for:

> **Secure Shell**

SSH uses a pair of cryptographic keys:

```text
Public Key  → Can be shared with GitHub
Private Key → Must be kept secure on your local machine
```

The two keys are mathematically related.

---

# Why Do We Need SSH Keys?

Suppose I want to push my code from my local machine to GitHub:

```text
Local Machine
     │
     │ git push
     ↓
   GitHub
```

GitHub needs to verify:

> "Is this computer authorized to access this GitHub account/repository?"

SSH provides a secure way to authenticate the local machine.

We generate an SSH key pair locally and add the **public key** to GitHub.

The **private key stays on our computer**.

---

# Generate an SSH Key

A common command for generating an RSA SSH key is:

```bash
ssh-keygen -t rsa -b 4096 -C "your-email@example.com"
```

### Breaking down the command

```text
ssh-keygen
```

Generates an SSH key pair.

```text
-t rsa
```

Specifies the key type as RSA.

```text
-b 4096
```

Specifies the key size as 4096 bits.

```text
-C "your-email@example.com"
```

Adds a comment to help identify the key.

The email/comment does not itself authenticate you; it is primarily an identifier for the key.

---

# What Happens After Running `ssh-keygen`?

When we run:

```bash
ssh-keygen -t rsa -b 4096 -C "your-email@example.com"
```

the terminal asks where we want to save the key.

It may show something like:

```text
Enter file in which to save the key:
```

If we press **Enter**, SSH uses the default location.

It may then ask:

```text
Enter passphrase:
```

We can:

* Enter a passphrase for additional protection, or
* Leave it blank

Using a passphrase provides an additional layer of protection if someone gains access to the private key file.

---

# SSH Key Files

After generating the key pair, we get two files.

For example, if we name the key:

```text
testkey
```

we will have:

```text
testkey
testkey.pub
```

### `testkey.pub`

The `.pub` file is the **public key**.

```text
testkey.pub → Public Key
```

The public key can be added to GitHub.

It is designed to be shared.

---

### `testkey`

The file without `.pub` is the **private key**.

```text
testkey → Private Key
```

This key must remain private.

**Never upload or share your private key.**

Do not put it in:

* GitHub repositories
* Screenshots
* Chat messages
* Public websites
* Project folders that are committed to Git

---

# How to Find the SSH Key

If the key was created in the current directory, we can list files using:

```bash
ls
```

Or search for a particular key name:

```bash
ls | grep testkey
```

You might see:

```text
testkey
testkey.pub
```

Remember:

```text
testkey     → Private Key 🔒
testkey.pub → Public Key
```

---

# Add the Public Key to GitHub

Go to GitHub:

```text
GitHub
   ↓
Profile Picture
   ↓
Settings
   ↓
SSH and GPG keys
   ↓
New SSH key
```

Give the key a recognizable title, for example:

```text
Rishabh's MacBook Air
```

Then copy the contents of:

```text
testkey.pub
```

and paste it into the SSH key field.

---

# How SSH Authentication Works

The basic idea is:

```text
                  GitHub
                    │
             Public Key Stored
                    │
                    │
Local Machine       │
──────────────      │
Private Key 🔒 ─────┘
```

When the local machine connects to GitHub using SSH, GitHub can verify that the machine possesses the private key corresponding to the public key that was registered with GitHub.

The private key itself is **not sent to GitHub**.

Instead, cryptographic authentication proves possession of the corresponding private key.

---

# Public vs Private Key

| Key         | Location               | Can it be shared? |
| ----------- | ---------------------- | ----------------- |
| Public Key  | GitHub + local machine | ✅ Yes             |
| Private Key | Local machine          | ❌ Never share     |

Easy way to remember:

```text
PUBLIC  → GitHub can have it
PRIVATE → Only you should have it
```

---

# Test the SSH Connection

After adding the public key to GitHub, test the connection using:

```bash
ssh -T git@github.com
```

The first time, you may see a message asking whether you want to continue connecting.

Type:

```text
yes
```

If authentication is successful, GitHub will respond with a message indicating that you have successfully authenticated.

---

# Using SSH with Git

When cloning a repository, GitHub provides an SSH URL such as:

```bash
git clone git@github.com:USERNAME/SDET.git
```

Instead of the HTTPS form:

```bash
git clone https://github.com/USERNAME/SDET.git
```

You can also configure your existing repository to use SSH as its remote:

```bash
git remote set-url origin git@github.com:USERNAME/SDET.git
```

Check it with:

```bash
git remote -v
```

You should see something similar to:

```text
origin  git@github.com:USERNAME/SDET.git (fetch)
origin  git@github.com:USERNAME/SDET.git (push)
```

---

# HTTPS vs SSH

GitHub can be accessed using either HTTPS or SSH.

### HTTPS

```text
https://github.com/USERNAME/SDET.git
```

### SSH

```text
git@github.com:USERNAME/SDET.git
```

SSH is commonly used by developers because, once properly configured, it allows Git operations to authenticate using the SSH key rather than repeatedly entering credentials.

---

# Complete SSH Setup Flow

```text
1. Generate SSH key pair
          ↓
2. Private key stays on computer 🔒
          ↓
3. Copy public key
          ↓
4. Add public key to GitHub
          ↓
5. Test SSH connection
          ↓
6. Configure Git remote to use SSH
          ↓
7. git pull / git push
```

---

# Important Security Rules

### Never share your private key

Never share:

```text
testkey
```

### Public key is okay to share

You can provide:

```text
testkey.pub
```

to GitHub.

### Never commit private keys

Your `.gitignore` should prevent accidental commits of sensitive files.

For example:

```gitignore
*.pem
id_rsa
id_ed25519
```

---

# Useful Commands

Generate RSA key:

```bash
ssh-keygen -t rsa -b 4096 -C "your-email@example.com"
```

List files:

```bash
ls
```

Search for a key:

```bash
ls | grep testkey
```

Test GitHub SSH:

```bash
ssh -T git@github.com
```

Check remote:

```bash
git remote -v
```

Change remote from HTTPS to SSH:

```bash
git remote set-url origin git@github.com:USERNAME/SDET.git
```

---

# Quick Mental Model

```text
                 GITHUB
            ┌──────────────┐
            │ Public Key   │
            └──────┬───────┘
                   │
             Authentication
                   │
                   ↓
            ┌──────────────┐
            │ Private Key  │
            │   🔒 LOCAL   │
            └──────────────┘
```

The important rule:

> **Public key goes to GitHub. Private key stays private on your machine.**
