# Dot files

## Managing dot files using GNU stow

It is really simple managing dotfiles using GNU stow.

## How it works

---
Stow manages dot files using symbolic links. Create a `dotfiles` directory in `$HOME` directory to keep all the dotfiles to be managed by stow.

Each dot file in the `~/dotfiles/<dotfile_name>` directory must have the same directory structure as the dotfile from `~`. For example,
if `.zshrc` is in `~`, then it must be located at `~/dotfiles/zsh/.zshrc`.

So, we want to backup `~/.tmux.conf` then in the `~/dotfiles` directory, it would be located at `~/.dotfiles/tmux`. Feel fee to name the directories as you please. Directory names are not important,just that the files in the directory must be at the same location from `$HOME` (`~`) directory.

## Limitations

A limitation of stow is that is does not support multiple profiles. But that can be a workaround by just creating a unique repo for all "different" computers.

## TL;DR

```shell
md ~/dotfiles ~/dotfiles/zsh ~/dotfiles/tmux ~/dotfiles/p10k

cp ~/.zshrc ~/.zshrc.backup
cp ~/.tmux.conf ~/.tmux.conf
cp ~/.p10k.zsh ~/.p10k.zsh.backup

mv ~/.zshrc ~/dotfiles/zsh
mv ~/.tmux.conf ~/dotfiles/tmux
mv ~/.p10k.conf ~/p10k

cd ~/dotfiles

stow zsh
stow tmux
stow p10k

touch .stow-local-ignore
echo ".git" >> ".stow-local-ignore"
echo ".gitignore" >> ".stow-local-ignore"

touch .stow-global-ignore
echo "/.DS_Store" >> .stow-global-ignore

git init
git add .
git commit -m "Added dot files with stow"
```

## Step By Step Guide

---

{{% steps %}}

### Install stow

Download and install GNU stow.

```shell
sudo apt install stow
```

### Create `~/dotfiles` directory

Create a directory called `dotfiles` in home directory `~`.

```shell
md ~/dotfiles
```

### Backup existing dotfiles

Now, to backup `~/.zshrc`, first create a backup of the same file.

```shell
cp ~/.zshrc ~/.zshrc.backup
```

Create a directory called `zsh` inside `/dotfiles` where `.zshrc` will live.

```shell
md ~/.zshrc
```

Then, move the `~/.zshrc` to `~/dotfiles/zsh`.

```shell
mv ~/.zshrc ~/dotfiles/zsh/.zshrc
```

### Move dotfiles to appropriate directories in `~/dotfiles`

Do this process for all the dot files that you want to manage.

```shell
cp ~/.tmux.conf ~/.tmux.conf.backup
mv ~/.tmux.conf ~/dotfiles/tmux

cp ~/.p10k.zsh ~/.p10k.zsh.backup
mv ~/.p10k.zsh ~/dotfiles/p10k

cp ~/.tmux.conf ~/.tmux.conf.backup
mv ~/.tmux.conf ~/dotfiles/tmux
```

### Directory structure of `~/dotfiles`

The directory structure should look something like this. There is a file called `.stow-local-ignore`, more about that later.

{{< filetree/container >}}
    {{< filetree/folder name="dotfiles" >}}
        {{< filetree/folder name="zsh" >}}
            {{< filetree/file name=".zshrc" >}}
        {{< /filetree/folder >}}
        {{< filetree/folder name="tmux">}}
            {{< filetree/file name=".tmux.conf" >}}
        {{< /filetree/folder >}}
        {{< filetree/folder name="p10k">}}
            {{< filetree/file name=".p10k.zsh" >}}
        {{< /filetree/folder >}}
    {{< filetree/file name=".stow-local-ignore" >}}
    {{< filetree/file name=".stow-global-ignore" >}}
    {{< /filetree/folder >}}
{{< /filetree/container >}}

### Have stow create the symbolic links

Now, from the `~/dotfiles` directory, run `stow <directory_name>`, for example, `stow zsh` for `.zshrc`. Stow will automatically create a symbolic link to `~/.zshrc` from `~/dotfiles/.zshrc`.

```shell
cd ~/dotfiles

stow zsh
stow p10k
stow tmux
```

### Initialize git repo

Initialize a git repo.

```shell
git init
```

### Add files and directories to `.stow-local-ignore` and `.stow-global-ignore`

{{< callout emoji="❓" >}}
  Add any files or directories that you want to ignore in the `.stow-local-ignore` and `.stow-global-ignore` files. For example, you can add `.git` to `.stow-local-ignore` so that stow ignores the git directory.
{{< /callout >}}

`.stow-local-ignore` file is like a `.gitignore` file for stow. `.stow-local-ignore` ignores per package (directory) and `.stow-global-ignore` ignores files and directories globally.

We will add `.git` to `.stow-local-ignore` so the stow ignores that directory.

```markdown
.git
.gitignore
```

### Push to github

Push to a github repo.

```shell
git push origin main
```

{{% /steps %}}

