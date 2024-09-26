---
title: win11 wsl2 ubuntu setup for multiple git accounts
created: '2024-09-25T00:00:53.951Z'
modified: '2024-09-26T15:09:31.912Z'
---

# win11 wsl2 ubuntu setup for multiple git accounts

useful for having multiple git accounts, I have one for work and one for personal use. Also works on win11 only and probably on the rest of wsl2 distros.

## 1. Generating a new SSH key and adding it to the ssh-agent:

```bash
ssh-keygen -t ed25519 -C "juandelacruz@personal.com"
```
```bash
ssh-keygen -t ed25519 -C "juandelacruz@work.com"
```

start the agent:
```bash
eval "$(ssh-agent -s)"
```
add both to ssh agent:
```bash
ssh-add ~/.ssh/id_ed25519
```
```bash
ssh-add ~/.ssh/for_work_id_ed25519
```
readmore: https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent

## 2. Adding a new SSH key to your GitHub account:

Add the ssh public keys to the github accounts:

https://docs.github.com/en/authentication/connecting-to-github-with-ssh/adding-a-new-ssh-key-to-your-github-account#adding-a-new-ssh-key-to-your-account

## 3. Creating the SSH Configuration File for multiple github accounts:
```bash
nano ~/.ssh/config
```
```
Host github.com-personal
    HostName github.com
    User git
    IdentityFile ~/.ssh/id_ed25519

Host github.com-work
    HostName github.com
    User git
    IdentityFile ~/.ssh/for_work_id_ed25519
```
save the changes and exit the text editor.

## 4. When cloning a repo use the alias:
```bash
git clone git@github.com-personal:syntaxtrash/nb.git

```
```bash
git clone git@github.com-work:syntaxtrash/nb.git

```
This will use the `~/.ssh/id_ed25519`

## 5. Update git config:

My setup is I use the global scope config for my personal info, and local (repository-specific) config for work. Use the setup that works for you.

global scrope config:
```bash
git config --global user.name "Juan Pogi"
```
```bash
git config --global user.email "juan@personal.com"
```

(repository-specific) config:
```bash
git config user.name "Juan DC."
```
```bash
git config user.email "juan@work.com"
```

Done.

## (Optional) Generating a GPG key:
https://docs.github.com/en/authentication/managing-commit-signature-verification/generating-a-new-gpg-key

I also generate gpg keys for each accounts.
```bash
gpg --full-generate-key
```

```bash
gpg --list-secret-keys --keyid-format=long

# From the list of GPG keys, copy the long form of the GPG key ID you'd like to use. In this example, the GPG key ID is 3AA5C34371567BD2::
------------------------------------
sec   4096R/3AA5C34371567BD2 2016-03-10 [expires: 2017-03-10]
uid                          Hubot <hubot@example.com>
ssb   4096R/4BB6D45482678BE3 2016-03-10
```

```bash
gpg --armor --export 3AA5C34371567BD2
# Prints the GPG key ID, in ASCII armor format
```

Adding the generated gpg key to git config:
```bash
git config --global user.signingkey 3AA5C34371567BD2
```
## Add to github accout:
https://docs.github.com/en/authentication/managing-commit-signature-verification/adding-a-gpg-key-to-your-github-account

## Signing commits with gpg key:
Add the `-S` flag:
```bash
git commit -S -m "my changes"
```

This will prompt you the passphrase.

## Important: 


In WSL2, the terminal environment can be slightly different from a standard Linux setup, so it's important to explicitly tell GPG where to prompt for passphrases. 

1. Open `.bashrc` in WSL2:
```bash
nano ~/.bashrc
```
2. Add these lines at the bottom of the file:
```bash
GPG_TTY=$(tty)
export GPG_TTY
```
3. Save the file and reload .bashrc:
```bash
source ~/.bashrc
```

GPG needs to know which terminal (TTY) to interact with when it needs to prompt you for a passphrase or other input. Without this variable set, GPG might not know where to display the prompt, especially in environments like WSL2 or remote terminals, leading to failures like "no terminal" or "bad file descriptor" errors.

