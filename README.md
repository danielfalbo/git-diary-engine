# Use git as a diary engine

Journaling via messages of empty commits

Inspired by [Notetime](https://notetimeapp.com)

## **`.zshrc`/`.bashrc` config**

1. Create a new git repo for your journal

2. Set `JOURNAL_DIR` to its path

```bash
export JOURNAL_DIR=$HOME/Developer/j
```

3. Alias `cdj` to (safely) navigate into it

```bash
alias cdj='[ -z "$JOURNAL_DIR" ] && echo "export JOURNAL_DIR first" && return 1 || cd $JOURNAL_DIR'
```

4. Create a function to write a note in vim and use it as message for an empty commit

```bash
function empty_commit() {
    TMPFILE=".tmp"
    vim -c ":set nocursorline" -c "Goyo" -c "startinsert" "$TMPFILE"
    [ -s "$TMPFILE" ] && git commit --allow-empty -m "$(cat "$TMPFILE")"
    rm "$TMPFILE"
}
```

5. Now alias `j` to create a quick journal entry from wherever you are in your shell

```bash
alias j='cdj && empty_commit'
```

6. Scroll the entries as pretty git logs with

```bash
alias glog='git log --pretty=format:"%C(240)%ad%Creset %s%n%b" --date=format:"%Y-%m-%d %I:%M:%S%p"'
```

https://github.com/user-attachments/assets/4a4d0f07-9a7a-45c6-b25a-e67ac2258a63
