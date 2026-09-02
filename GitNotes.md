# Git Commands — My Notes

## The Shipping Metaphor

```
git init      →  Build the warehouse (turn a plain folder into a Git repo)
[edit files]  →  Items sitting on your desk (working directory) — on your PC/laptop
git add       →  Load chosen items into the van (staging area)
git commit    →  Van departs, delivery is permanently logged (repository history)
```

| Command | Meaning |
|---|---|
| `git status` | Right now — what's on your desk (unstaged) vs. what's currently loaded in the van (staged), before anything ships |
| `git log` | History — every shipment that has already departed (every past commit) |

- When running `git log`, it will also show `(HEAD -> main)`, which indicates which branch or which committed state you are on. It's like a stack, where the last or newly committed point is the HEAD/main.
- When running `git log`, it will also open a pager for scrolling through commits if there are multiple/larger commits. When `(END)` is shown, press `q` to quit out of the pager/log.

---

## `git show` — just look, nothing changes

```bash
git show 663da2cbdfd774d6529ad1b4c7905da9f635035c:chapter3.txt
```

- Prints what `chapter3.txt` looked like at that specific commit, to the terminal.
- Nothing on your desk (working directory) or in the van (staging area) is touched.
- Equivalent to: reading the old packing slip/delivery record for a specific past shipment — purely informational.

To find the right commit hash for a file first:
```bash
git log chapter3.txt
```

---

## `git restore` — bring an old version back to your desk

```bash
git restore --source=663da2cbdfd774d6529ad1b4c7905da9f635035c chapter3.txt
```

- Takes the version of `chapter3.txt` from that old commit and overwrites the current file in your working directory with it.
- Does not touch commit history — the old commit and all commits since remain unchanged.
- After running this, `git status` will show `chapter3.txt` as modified (it now differs from the latest commit).
- If you want this restored version to become the new "current" version going forward, you still need to `git add chapter3.txt` + `git commit` again.
- Equivalent to: pulling an old, already-shipped item back out of the archive and putting it back on your desk to work with again.

Older equivalent syntax (same effect, predates `restore`):
```bash
git checkout 663da2cbdfd774d6529ad1b4c7905da9f635035c -- chapter3.txt
```

---

## `git revert` — cancel out a commit's effect with a new commit

```bash
git revert 663da2cbdfd774d6529ad1b4c7905da9f635035c
```

- Does not delete or erase the old commit — the accidental commit stays permanently in history, exactly as it was.
- Creates a brand-new commit whose changes are the exact opposite of the target commit — so the net effect (your files, right now) look as if the accidental commit's changes were undone.
- End result: both commits exist side by side in history forever — the original accidental one, and the new "undo" one right after it.
- Git will open your default editor to confirm/edit the auto-generated commit message. To skip that prompt for a simple case:
```bash
git revert --no-edit 663da2cbdfd774d6529ad1b4c7905da9f635035c
```
- Safe to use even after pushing to GitHub — it doesn't rewrite shared history, it only adds to it.
- Equivalent to: sending a new "correction" shipment that says "disregard/undo what was in that earlier shipment" — the effect is canceled, but the original shipment's record is untouched and still visible.

---

## `git checkout` — multi-purpose command (branches AND files)

`checkout` predates `restore`/`switch` and does several different jobs depending on how it's used — this is why it can be confusing.

### a) Switching branches
```bash
git checkout branch-name
```
Moves you to a different branch — like switching which project timeline/delivery route you're currently working on.

### b) Creating + switching to a new branch in one step
```bash
git checkout -b new-branch-name
```
Equivalent to:
```bash
git branch new-branch-name
git checkout new-branch-name
```

### c) Restoring a file to an old version (same effect as `git restore`)
```bash
git checkout 663da2cbdfd774d6529ad1b4c7905da9f635035c -- chapter3.txt
```
Pulls `chapter3.txt`'s content from that old commit back onto the working directory — identical result to `git restore --source=...`.

---

## `git branch`

- `git branch` — gives what current branch we're in.
- `git branch "new-branch"` — a branch is just another route/timeline for the same van, starting from wherever you currently are, and from that point onward it keeps its own separate list of stops (commits) — completely independent of whatever the original route does afterward.
- `git checkout "new-branch"` will switch to this new branch, and so will `git checkout -b "new-branch"` (creates + switches in one step).

---

## `git remote`

- `git remote` — in shipment terms: "I will give you the address to deliver or transfer the shippings/product/code to this remote address or known warehouse."
- `git remote add` — `add` simply means: "add this address" / "this is the address for that warehouse or remote address."
- `git remote add origin <url>` (or any string name) — the nickname you're choosing for this address. It's not required or a reserved keyword.
- `git remote -v` — show me my saved address book (all remotes, with their nicknames and URLs).

---

## `git rm`

```bash
git rm filename1 filename2 ... filenameN
```
Removes files locally and in remote — both together — then `git add` and `git commit` (to ship the removal).

To remove only from remote (keep local files):
```bash
git rm --cached filenameN
```
