## Simple git server example

### Walkthrough using a host running FreeBSD

```sh
su && pkg install git
adduser # git, set home directory to /srv/git
su git
mkdir -p ~/.ssh && chmod 700 ~/.ssh
touch ~/.ssh/authorized_keys && chmod 600 ~/.ssh/authorized_keys
git init --bare /srv/git/test.git # initialize a repository holding only the compressed data


# on client, generate SSH key pair if you don't already have one
ssh-keygen -t ed25519
ssh-copy-id git@<host IP> # add the public key to the git user's authorized_keys file; authenticate with password


# optional security improvements

# force authentication with keys
sed -E -i '' 's/^#PasswordAuthentication no/PasswordAuthentication no/' /etc/ssh/sshd_config
echo "ChallengeResponseAuthentication no" >> /etc/ssh/sshd_config
echo "PermitEmptyPasswords no" >> /etc/ssh/sshd_config

# further secure the git user by restricting its shell
su && chsh -s /usr/local/bib/git-shell git


git clone git@<host IP>:test.git # clone the repo with SSH URL # finally, clone the repository
```
