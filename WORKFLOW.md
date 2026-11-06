# Workflow

How to work on the submodules and keep this repo's pins up to date.

## Enable the push guard (once per clone)

This repo ships a `pre-push` hook (`.githooks/pre-push`) that blocks a push
if any submodule pin in `HEAD` points at a commit that isn't on that
submodule's own remote. Without it, a pin to an unpushed commit breaks
`git submodule update` for everyone else. Git doesn't track hook activation,
so enable it once per clone:

```bash
git config core.hooksPath .githooks
```

If a push is blocked, push the named submodule first.

## Branches

| Submodule | Branch | Notes |
|---|---|---|
| `nexus-lounge` | `master` | |
| `enigma-bbs` | `atproto-login` | A fork. `master` mirrors upstream ENiGMA½ and is never committed to; work is rebased onto upstream regularly. `pr/<nn>-<name>` tags mark where each planned upstream PR is complete. Never push to upstream |

Submodules check out a detached commit. Before changing one, switch to its
branch:

```bash
git -C enigma-bbs switch atproto-login
```

## Updating a pin

1. Commit and **push the submodule first**:
   `git -C <submodule> push origin <branch>`
2. Then record the new pin here:

   ```bash
   git add <submodule>
   git commit -m "Pin <submodule> at <what changed>"
   git push
   ```

The hook enforces the order.

To move every submodule to the tip of its branch:

```bash
git submodule update --remote
```

Review what moved (`git submodule status`, `git diff --submodule`) before
committing the new pins.

## Where development happens

The ENiGMA½ fork is developed and tested on a Linux VM, not in this
checkout. This repo records which commits of each project go together; the
submodule checkouts here are for reading and for moving pins.
