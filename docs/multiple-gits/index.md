# Managing Multiple Gits and Github Accounts

## Problem

Sometimes you just need to have more than one git accounts on the same computer. Most common use case is - one for personal use and one for work.

## Solution

Have separate ssh keys for accounts. Use global config for your personal (if you are on your personal computer) and override git config per repo for work.

{{% steps %}}

### Generate SSH keys for each account

- For personal account

```shell
ssh-keygen -t rsa -b 4096 -C "personal_email@example.com" -f ~/.ssh/id_rsa_personal
```

- For work account

```shell
ssh-keygen -t rsa -b 4096 -C "work_email@company.com" -f ~/.ssh/id_rsa_work

```

### Add SSH keys to respective Github accounts

1. Copy the contents of each public key file (e.g., `~/.ssh/id_rsa_personal.pub` and `~/.ssh/id_rsa_work.pub`).
2. Go to GitHub Settings > SSH and GPG keys on each respective account.
3. Add your SSH key as a new key for each account

### Configure ssh config file

```yaml {filename=config}
# Personal GitHub
Host github.com-personal
  HostName github.com
  User git
  IdentityFile ~/.ssh/id_rsa_personal
  IdentitiesOnly yes

# Work GitHub
Host github.com-work
  HostName github.com
  User git
  IdentityFile ~/.ssh/id_rsa_work
  IdentitiesOnly yes
```

This creates aliases (github.com-personal and github.com-work) to distinguish which identity is used when connecting.

### Use the correct aliases when cloning repos

- For personal account, use:

```shell
git clone git@github.com-personal:username/repo.git
```

- For work account, use:

```shell
git clone git@github.com-work:company/repo.git
```

### Configure Git user per repository

Set the Git user email and name per repository to ensure commits are associated with the correct account.

> [!IMPORTANT]
> This git repo must be initialized first.

```shell
cd path/to/your/repo
git config user.name "Your Name"
git config user.email "your_email@example.com"
```

> [!NOTE]
> This should be done inside each cloned repo so your identities don't conflict.

### Add SSH keys to ssh agent

To load your identities into the agent:

```shell
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_rsa_personal
ssh-add ~/.ssh/id_rsa_work
```

{{% /steps %}}

