# Cyberspace Framework

A stylized, practical interface to the internet that recalls the cyberspace
of cyberpunk fiction: a shared space you and other people move around in
while you use BBSes, chats, games, and VR worlds. Identity comes from the
[AT Protocol](https://atproto.com) (Bluesky accounts and others).

It is built in stages, starting with text terminals. The long-range design
is in [docs/vision.md](docs/vision.md).

## Projects

This repo holds framework-wide docs and decisions. The projects themselves
are separate repos, included here as git submodules:

| Submodule | Repo (branch) | What it is |
|---|---|---|
| `nexus-lounge/` | [zerosquaredio/nexus-lounge](https://github.com/zerosquaredio/nexus-lounge) (`master`) | The first piece: a BBS network with AT Protocol login, and its hub (directory and tunnels). Specs and plans, and later the hub's code |
| `enigma-bbs/` | [zerosquaredio/enigma-bbs](https://github.com/zerosquaredio/enigma-bbs) (`atproto-login`) | Fork of [ENiGMA½](https://github.com/NuSkooler/enigma-bbs) adding AT Protocol login, developed to be offered upstream |

Each submodule is pinned at a commit known to work with the others.

## Getting it

```bash
git clone --recurse-submodules https://github.com/zerosquaredio/cyberspace-framework.git
```

If you cloned without `--recurse-submodules`:

```bash
git submodule update --init --recursive
```

Working on the submodules and updating their pins: [WORKFLOW.md](WORKFLOW.md).

## Terms

| Term | Meaning |
|---|---|
| **Leaf** | Any endpoint people connect to through a hub: a BBS today; later chats, games, VR worlds |
| **Hub** | A public server leaves register with: login point, directory, tunnel relay, and common space |
| **Common space** | Where people on a hub meet, whatever leaf they are using |

More in [docs/vision.md](docs/vision.md) §2.
