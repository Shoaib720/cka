# Vim Cheatsheet for CKA Exam

> Focus: Speed, precision, and YAML editing under exam pressure.

---

## Essential Setup (Run First in Every Terminal)

```bash
# Paste this into every new terminal session
cat >> ~/.vimrc << 'EOF'
set number
set expandtab
set tabstop=2
set shiftwidth=2
set autoindent
set paste
EOF
```

> ⚠️ `set paste` disables auto-indent when pasting — toggle it off (`set nopaste`) if indentation stops working while typing.

---

## Modes

| Key | Action |
|-----|--------|
| `i` | Insert mode (before cursor) |
| `a` | Insert mode (after cursor) |
| `o` | New line below + insert |
| `O` | New line above + insert |
| `Esc` | Return to Normal mode |
| `v` | Visual mode (character) |
| `V` | Visual mode (line) |
| `Ctrl+v` | Visual block mode |

---

## Navigation

| Key | Action |
|-----|--------|
| `h j k l` | ← ↓ ↑ → |
| `w` / `b` | Next / previous word |
| `0` | Start of line |
| `$` | End of line |
| `gg` | Top of file |
| `G` | Bottom of file |
| `:<n>` | Go to line number (e.g. `:15`) |
| `Ctrl+d` | Scroll down half page |
| `Ctrl+u` | Scroll up half page |

---

## Editing

| Key | Action |
|-----|--------|
| `x` | Delete character under cursor |
| `dd` | Delete (cut) current line |
| `<n>dd` | Delete n lines (e.g. `3dd`) |
| `yy` | Yank (copy) current line |
| `<n>yy` | Yank n lines (e.g. `5yy`) |
| `p` | Paste below |
| `P` | Paste above |
| `u` | Undo |
| `Ctrl+r` | Redo |
| `cw` | Change word |
| `C` | Change from cursor to end of line |
| `r<char>` | Replace single character |
| `~` | Toggle case of character |

---

## Indentation (Critical for YAML)

| Key | Action |
|-----|--------|
| `>>` | Indent line right |
| `<<` | Indent line left |
| `<n>>>` | Indent n lines (e.g. `3>>`) |
| `=G` | Auto-indent from cursor to end of file |
| `gg=G` | Auto-indent entire file |

**Visual mode indentation:**
1. Press `V` to select lines
2. Press `>` to indent, `<` to unindent
3. Press `.` to repeat the last indent operation

---

## Search and Replace

| Command | Action |
|---------|--------|
| `/word` | Search forward |
| `?word` | Search backward |
| `n` | Next match |
| `N` | Previous match |
| `:%s/old/new/g` | Replace all occurrences in file |
| `:%s/old/new/gc` | Replace with confirmation |
| `:s/old/new/g` | Replace on current line only |

---

## File Operations

| Command | Action |
|---------|--------|
| `:w` | Save |
| `:wq` or `ZZ` | Save and quit |
| `:q!` | Quit without saving |
| `:w <filename>` | Save as new file |
| `:e <filename>` | Open a file |

---

## Working with Multiple Lines (YAML Workflows)

### Duplicate a block
1. `V` → select lines → `y` → `p`

### Comment out multiple lines
1. `Ctrl+v` → select lines (j/k) → `I` → type `#` → `Esc`

### Remove leading characters (e.g. `#`)
1. `Ctrl+v` → select lines → `x`

### Add same text to start of multiple lines
1. `Ctrl+v` → select lines → `I` → type text → `Esc`

---

## CKA-Specific Workflows

### Edit a YAML file from `kubectl`
```bash
kubectl edit deployment my-deploy
# Opens in vim automatically — edit, then :wq to apply
```

### Create a resource YAML quickly
```bash
kubectl run nginx --image=nginx --dry-run=client -o yaml > pod.yaml
vim pod.yaml
```

### Fix indentation after pasting
```
:set nopaste    # turn off paste mode
gg=G            # re-indent entire file
```

### Jump to a specific field
```
/containerPort    # search for field
n                 # next occurrence
```

### Delete all lines from cursor to end
```
dG
```

### Delete all lines from start to cursor
```
dgg
```

---

## Quick Reference Card

```
MODES:    i=insert  v=visual  V=line  Ctrl+v=block  Esc=normal

MOVE:     gg=top  G=bottom  0=line-start  $=line-end  :<n>=goto-line

EDIT:     dd=cut-line  yy=copy-line  p=paste  u=undo  Ctrl+r=redo

INDENT:   >>=indent  <<=unindent  V+>=visual-indent

SEARCH:   /word  n=next  N=prev  :%s/old/new/g=replace-all

SAVE:     :w=save  :wq=save+quit  :q!=force-quit
```

---

## Common Mistakes to Avoid

- **Wrong indentation in YAML** → Use `set expandtab tabstop=2 shiftwidth=2` (spaces, not tabs)
- **Paste breaks YAML structure** → Toggle `set paste` before pasting, `set nopaste` after
- **Accidentally exiting without saving** → Use `:wq`, not just `:q`
- **Can't type after `Esc`** → You're in Normal mode — press `i` to go back to Insert
- **Changes not applied** → In `kubectl edit`, ensure `:wq` (not `:q!`)