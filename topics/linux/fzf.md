# fzf (Fuzzy Finder)

Download

```bash
git clone --depth 1 https://github.com/junegunn/fzf.git ~/.fzf
```

Install

```bash
~/.fzf/install
```

Add this line to `~/.bashrc`.

```
[ -f ~/.fzf.bash ] && source ~/.fzf.bash
```

# fzf-cd

Navigate folders with `CTRL + f`.

Add this to `.bashrc`.

```bash
fzf_cd() {
    dir=$(find /mnt/c/user/dev/lab \( -name "node_modules" -o -name ".git" -o -name "android" \) -prune -o -type d -print | fzf --no-preview)
    cd "$dir"
}

bind -x '"\C-f": "fzf_cd"'
```
