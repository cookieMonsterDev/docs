# Multiple GitHub Accounts on One Machine (SSH + Auto Identity Switching)

1. Create folder structure

```bash
mkdir -p ~/projects/work
mkdir -p ~/projects/personal
```

2. Generate SSH keys

```bash
ssh-keygen -t ed25519 -f ~/.ssh/id_rsa_work -C "your_work_email@company.com"
```

```bash
ssh-keygen -t ed25519 -f ~/.ssh/id_rsa_personal -C "your_personal_email@gmail.com"
```

3. Add SSH keys to GitHub

```bash
~/.ssh/id_rsa_work.pub
```

```bash
~/.ssh/id_rsa_personal.pub
```

4. Configure SSH to use different keys. Create or edit `~/.ssh/config`:

```yml
# WORK GITHUB
Host github.com-work
  HostName github.com
  User git
  IdentityFile ~/.ssh/id_rsa_work

# PERSONAL GITHUB
Host github.com-personal
  HostName github.com
  User git
  IdentityFile ~/.ssh/id_rsa_personal
```

5. Remove global Git identity

```bash
git config --global --unset user.name
git config --global --unset user.email
```

6. Add conditional Git configs to `~/.gitconfig`

```yml
[includeIf "gitdir:~/projects/work/"]
    path = ~/.gitconfig-work

[includeIf "gitdir:~/projects/personal/"]
    path = ~/.gitconfig-personal
```


7. Create identity files 

`~/.gitconfig-work`

```yml
[user]
    name = Work Name
    email = work_email@company.com
```

`~/.gitconfig-personal`

```yml
[user]
    name = Personal Name
    email = your_personal@gmail.com
```

8. Clone repositories using SSH host alias

Personal repos:

```bash
git clone git@github.com-personal:YOUR_USERNAME/your-repo.git
```

Work repos:

```bash
git clone git@github.com-work:COMPANY_ORG/work-repo.git
```

# Result

* `~/projects/work/**` → Work name/email + work SSH key

* `~/projects/personal/**` → Personal name/email + personal SSH key

* No switching configs manually

* Everything isolated and automatic