# Cyberspace Framework: Vision

Status: Vision (draft)
Last updated: 2026-11-08

This is the long-range picture that the child projects build toward. It
records direction and decisions, not a schedule: features become plans in
the children (for Nexus Lounge, its
[development plan](https://github.com/zerosquaredio/nexus-lounge/blob/master/docs/development_plan.md)
and
[hub development plan](https://github.com/zerosquaredio/nexus-lounge/blob/master/docs/hub_development_plan.md))
only when they are near.

Links below to `nexus-lounge` specs point at its `master` branch.

## 1. Goal

A stylized, practical interface to the internet that recalls the cyberspace
of cyberpunk fiction: a **shared space** you and other people move around in
while you use BBSes, chats, games, and VR worlds. It is built in stages,
each useful on its own, from text terminals today toward 3D and VR spaces.

Nexus Lounge (BBSes plus a hub) is the first piece of it.

**Easy by default, better with effort.** Starting a leaf should take no
networking knowledge: a ready-made image, an outbound tunnel to a hub, no
open ports, domains, or certificates, and it may be up only part of the
time. Admins who are comfortable doing more can turn on more efficient
options as they go, such as direct ports for SSH or telnet, their own
public address, or a direct source for their assets, and each such option
makes the whole system work better (less load on hubs, faster
connections). The default path always works on its own; the options are
improvements, never requirements (§5.1).

**The hacker ethos.** Things should work, and common users need a way to
authenticate (AT identity); beyond that, everything should be open source,
modifiable, and extendable. People should be able to take it apart, change
it, and build on it, from running their own leaf or hub to programming
things inside the shared space (§8.1). This is also why the core of each
layer must be software we fully control and can fork (hub spec §5.3).

## 2. Terms

| Term | Meaning |
|---|---|
| **Leaf** | Any endpoint people connect to through a hub: a BBS today; later chats, games, VR worlds, other services. Run by its own admin (for a BBS, the sysop) |
| **Hub** | A public server that leaves register with. It is a login point and directory for users, can relay traffic to leaves through tunnels, and can host a common space ([hub spec](https://github.com/zerosquaredio/nexus-lounge/blob/master/docs/hub.md)) |
| **Common space** | Where people on a hub meet, whatever leaf they are using: text, voice, avatars, and later 3D environments |
| **Client tier** | What a person's client can do: web page, desktop app, VR headset. Everyone takes part at the level their client supports |

### 2.1 What goes over AT

AT Protocol is a general network: identity, repositories of records, and
sync between servers. Bluesky is one application on it, with its own record
types (`app.bsky.*`); the framework defines its own types the same way and
needs nothing added to AT.

AT records are **public, permanent, signed, and broadcast** to anyone
following the network. That decides what goes in them:

| Kind of data | Carried by | Examples |
|---|---|---|
| Lasting public facts | AT records | Leaf listings, hub offers and agreements, game catalogues, avatar wardrobes (§4.1, §7, §9.2) |
| Private, live, or point-to-point | The framework's own transport: tunnels, WebRTC, hub services | Terminal sessions, chat, voice, presence, lobbies, game networks, per-leaf avatar choices (§5, §7.2, §8) |

Bluesky made the same split: its DMs are a separate chat service, not AT
records. Users can still call hub services as themselves without a separate
login, through their PDS's service proxying
([hub spec](https://github.com/zerosquaredio/nexus-lounge/blob/master/docs/hub.md)
§5.2).

### 2.2 Record type names

Decided 2026-11-06. Record types use the NSID root **`nexus.cyberspace`**,
from the framework's domain `cyberspace.nexus`. The domain belongs to the
framework rather than to a person, so it can be handed to a community. Names
are grouped by **whose repo the record lives in**:

| Group | Lives in | Records so far |
|---|---|---|
| `nexus.cyberspace.leaf.*` | A leaf's repo (a BBS today) | `listing`, `status`, `stats`, `ad`, game catalogue entries, hub `request` and `agreement` |
| `nexus.cyberspace.hub.*` | A hub's repo | `offer`, `agreement`, `observedStatus`; also the hub's API methods (e.g. `listLeaves`, `getLeaf`) |
| `nexus.cyberspace.actor.*` | A user's repo | Avatar wardrobe and default, only if at3d.app's record types don't cover them (§7) |
| `nexus.cyberspace.dev.*` | Throwaway test accounts | Anything during development; never used for real data |

- Each group is its own Lexicon authority, so each needs a DNS TXT record
  (`_lexicon.leaf.cyberspace.nexus`, `_lexicon.hub.cyberspace.nexus`, and so
  on) naming the project account that publishes the schemas. These are added
  when the first schemas are published (Nexus Lounge F9), not before.
- New groups can be added later without affecting existing names.
- Names and fields are permanent once published; drafts and the change rules
  are in the [publishing spec](https://github.com/zerosquaredio/nexus-lounge/blob/master/docs/publishing.md) §3.
- Schemas will live in this repo's `lexicons/` directory, under CC0.
- The reference hub uses `hub.cyberspace.nexus`; other hubs use their own
  domains.

## 3. Layers and stages

| Stage | Hub provides | Users get |
|---|---|---|
| **1. Directory and tunnel** (planned: Nexus Lounge F10, F11) | Directory, handles, terminal and web traffic relayed to leaves behind firewalls | Find and connect to BBSes from one place, with one identity |
| **2. Common space, 2D** | Presence, text and voice chat, shared interfaces, game lobby (§9) | A web page showing who is around; chat while connected to one or more BBSes (tiled); optionally broadcast your screen to others; door and browser games with people across leaves |
| **3. Assets** | Avatars and icons, cached and relayed | Each person shown with their own avatar or ANSI art; new users get defaults |
| **4. 3D and VR** | Environments from the hub and from leaves; real-time relay where affordable | A 3D space in the browser, a desktop client, or a headset, with a pop-up "desktop" showing your BBS sessions, chat controls, and other apps |
| **5. Hub federation** | Hubs connected to each other | One larger space across hubs |

Everyone can be present at every stage their client supports: someone without
VR still appears (as a small avatar) and can chat with people who are in VR.

Leaves may also run their own private spaces (for example a VR-only leaf)
that people use separately from the hub's common space.

## 4. Hubs and leaves

### 4.1 Offers, requests, and agreements

The relationship between a hub and a leaf is a handshake, published as AT
records so anyone can check it:

| Record | In whose repo | Says |
|---|---|---|
| **Offer** | Hub | What the hub relays, bandwidth limits, theme or community, rules, terms (free, a suggested contribution, or a price) |
| **Request** | Leaf | The leaf's services, the traffic it expects, the offer terms it accepts |
| **Agreement** | Both: one record each, pointing at the other | The leaf is carried by the hub on these terms. Either side ends it by deleting its half |

Because offers are public records, a leaf admin can browse hubs by theme,
capabilities, or price without any central marketplace. Over time leaves are
expected to move to hubs with similar leaves, more capability, or lower cost
(§6.1).

### 4.2 Money

Agreements record terms, not payments. Payment happens outside the protocol,
through whatever the hub admin uses (Open Collective, Liberapay, Stripe, …);
handling money ourselves would bring refunds, fraud, tax, and payment
provider rules into the project.

Metering is the trust problem: a leaf paying toward bandwidth can't see what
the hub measured. To start, the hub publishes per-leaf usage summaries and
leaves compare them with their own logs. Stronger measures can come later.

### 4.3 Hub software

Any hub runs the same open software, and nothing ties a leaf to one hub. A
hub and a leaf may also run on the same machine.

## 5. Transport

### 5.1 Policy: as much as the hub can carry, configurable

Ideally everything a leaf serves goes through the hub, so leaf admins need no
open ports, domains, or certificates. Whether that is affordable depends on
the hub's location and budget, so it is a policy, set per hub and per leaf:

- **Hub admin:** which kinds of traffic the hub relays, bandwidth budgets,
  per-leaf quotas and exceptions.
- **Leaf admin:** whether to offer direct addresses at all. A leaf behind a
  firewall with no domain has only the hub's relay.
- What happens is where the two overlap. When neither path exists for a
  service (the hub won't relay it and the leaf has no direct address), the
  directory says the service is unavailable rather than letting clients fail.

This is a proven pattern: Tailscale relays through its servers when a direct
path isn't possible, and WebRTC falls back to a relay server (TURN).

### 5.2 Route tables

A hub hands out **routes, not just listings**. Each leaf's directory entry
lists its services (terminal, web, assets, real-time world connection) and,
for each, how to reach it: through the hub's relay, at a direct address, or
both, in order of preference. Clients always follow the route table, so a
leaf can switch between relayed and direct without any client change.

### 5.3 Handoff to a leaf

When a user moves into a leaf's own space, the hub can hand their
connection off: it gives the client a new route (connect directly to the
leaf) and a short-lived signed ticket, which the leaf checks (or the user
authenticates to the leaf by DID). The hub then drops out of that traffic.

This needs a leaf that offers a direct address and allows handoffs, and a
client that can connect directly. Otherwise the user stays relayed.

**Privacy:** a direct connection shows the user's IP address to the leaf;
relaying hides it. Users have a "keep me relayed" setting, honored even
when a handoff is possible.

### 5.4 Connections between users

The hub introduces users to each other and relays their traffic only when no
direct path works, or when either user asks to stay relayed (§5.3):

| Use | Mechanism | Clients |
|---|---|---|
| Text and voice chat | WebRTC: the hub signals, peers try a direct path through NATs, the hub relays (TURN) if that fails. Rooms of more than a few people use a media server (SFU) at the hub instead (§8) | Browsers and native |
| Games and other apps | An overlay network per session (as Tailscale, ZeroTier, Nebula): the hub coordinates (for example with [Headscale](https://github.com/juanfont/headscale)) and relays as a fallback. Participants see each other as if on one LAN | Native only; browsers can't join |

Rules for session networks:

- One network per session, containing only the people invited, ending when
  the session ends. Never a bridge into anyone's home network.
- Limited to the ports the app needs, where possible.
- Relaying arbitrary traffic is where abuse and legal exposure live, so it is
  a hub admin setting, with rate limits, off by default.

In VR, the game still runs on each user's PC; the VR environment shows it as
a window or panel (Overte has web and app panels). That is a client feature,
separate from the networking.

### 5.5 Traffic classes

| Class | Examples | Relaying |
|---|---|---|
| Terminal and web | SSH, telnet, WebSocket terminals, web pages | Cheap. The planned tunnel. SSH stays end-to-end encrypted; telnet and plain web are readable by the hub ([hub spec](https://github.com/zerosquaredio/nexus-lounge/blob/master/docs/hub.md) §7) |
| Assets | Avatars, icons, ANSI art, environments, game downloads | Affordable with caching: assets addressed by content hash (CID) never change, so the hub fetches each once and serves it to everyone |
| Real-time | Voice, VR positions and world state | Expensive, and needs a UDP-capable relay (TURN, QUIC, or WebTransport), because TCP tunnels add stutter. Optional per hub and per leaf |
| Between users | Chat, voice, game sessions (§5.4) | Small for chat, voice (around 50 kbit/s per person), and classic games; relayed only when direct fails |

Same-machine leaves need no tunnel: the hub proxies to a local socket.

## 6. Identity

### 6.1 Decision: every leaf has its own DID

*Proposed 2026-11-06.* This confirms the BBS account in Nexus Lounge's
[publishing spec](https://github.com/zerosquaredio/nexus-lounge/blob/master/docs/publishing.md)
§2 and extends it to every kind of leaf.

**A leaf is an AT account with its own `did:plc` DID, separate from its
admin's personal account, hosted on any PDS the admin chooses, with the admin
holding the DID's rotation key.**

Alternatives considered:

| Leaf identity | Works for firewalled leaves | Can move hubs | Problem |
|---|---|---|---|
| Hub-issued ID or address | Yes | No | The hub owns the identity; leaving means starting over |
| Its own domain | No | Yes | Excludes the admins tunnels are meant to help |
| A self-generated key pair | Yes | Yes | Reinvents DIDs, with no repo to publish records in |
| **Its own AT DID** | **Yes** | **Yes** | Needs an account on some PDS |
| The admin's personal DID | Yes | Yes | Mixes person and leaf; the leaf can't change hands; one admin's leaves collide |

What it commits us to:

- **`did:plc`, not `did:web`.** `did:web` is tied to a domain, which brings
  back the domain requirement.
- **Hosting is the admin's choice:** a Bluesky account created for the leaf
  (handle like `coolbbs.bsky.social`), the admin's own PDS, or a PDS the hub
  runs for its leaves (Nexus Lounge hub plan, H11). AT account migration
  means none of these locks the leaf in. If a hub hosts the account, the
  admin still holds the rotation key, so the hub can't keep the identity when
  the leaf leaves.
- **The leaf publishes.** It writes its listing, request, agreement, and
  game catalogue records, so it needs a write-capable OAuth session for its
  account (Nexus Lounge F9), beyond the identity-only login users get.
- **Hubs authenticate leaves by DID.** A tunnel proves it belongs to the
  leaf's DID (AT service auth, signed with the DID's key), and the hub checks
  that against the agreement (the hub spec, §1.2, already has tunnels
  authenticate as the BBS's DID).
- **Hub-issued handles and addresses are conveniences, not identity.** A
  leaf moving hubs keeps its DID; its handle can change.

What follows from it:

- **Migration:** a new agreement with the new hub, the leaf's directory entry
  updated, clients following the new routes. The old hub keeps a forwarding
  note for a grace period, like an HTTP 301. Users are bound to DIDs too, so
  their accounts on the leaf don't depend on the hub.
- **Multi-homing:** a leaf may be carried by more than one hub at once (say
  a themed hub and a high-bandwidth one), if the hubs' rules allow it.

### 6.2 User identity

AT accounts (Bluesky or any other PDS) are the first identity provider for
users, as on the BBS today. They are not required to be the only one: hubs
and leaves keep identity behind an interface so a better mechanism (passkeys,
another federated identity) can be added alongside. On the BBS this already
exists as login types (`users.allowedAuthFactor1Types`).

### 6.3 Joining: leaves and their existing users

*Decided 2026-11-06.* A leaf joining a hub does **not** require its users to
have AT accounts. Two things are separate:

- **The leaf joining** is its admin's job, done once: create the leaf's AT
  account (§6.1), optionally turn on AT login, and make an agreement with a
  hub (§4.1). The leaf then appears in the directory and its traffic can go
  through the hub's tunnel.
- **A user taking part in the hub's shared features** (presence, the common
  space, avatars, the game lobby) needs a hub-level identity.

Existing users lose nothing. A password user can still find the leaf in the
directory, connect through the hub's tunnel, and log in with their old
username and password; the hub routes the session without knowing who they
are.

Levels of participation:

| User | Can |
|---|---|
| **No account anywhere** (visitor) | Browse the directory, connect to leaves, apply on leaves that take new users |
| **Leaf account only** | Everything they could before, through the hub |
| **Leaf account, vouched for by the leaf** (§6.4) | Appear in the common space as `alice@coolbbs`, chat, join lobbies, as that leaf's user only |
| **AT account** | Full hub membership: one identity across all leaves, avatars from their own repo, presence, everything across leaves |

There is **no separate hub registration**: logging in to the hub with AT is
the hub account. The hub keeps a small profile keyed by DID, created at
first login, and stores no passwords.

**Linking an existing leaf account to AT** works from both directions, and
needs proof of both sides (the leaf password and the AT approval):

1. **From the leaf:** the user logs in with their password, chooses "link
   Bluesky account" in their settings, and approves in the browser. The
   leaf records the DID on their existing account; either login works after
   that.
2. **After AT login:** when a leaf finds no user for the DID, it offers "I
   already have an account here" (username and password, once) before the
   new user application.

The link is stored on the leaf, privately (§7.2's rule), not published.

**Lowering the barrier to an AT account:** a hub may run a PDS (already
planned for leaf accounts) that also creates *user* accounts, with handles
like `alice.hub.example`. "Sign up at the hub" then means getting a real,
portable AT account in one step.

### 6.4 Leaf-vouched identities

For users who don't want an AT account, the leaf vouches for them: it gives
the hub a short-lived statement ("this session is user 42, `alice`, on
coolbbs") signed with the leaf DID's key. The hub shows them as
`alice@coolbbs`.

- The hub trusts the leaf's word, and only for that leaf's users; a leaf
  can't vouch for anyone as belonging elsewhere.
- The identity doesn't carry to other leaves, and it has no AT repo, so
  avatars come from the leaf or the hub's defaults (§7.2, steps 2 and 5).
- These are signed tokens, not published records.
- Both sides opt in:
  - **The leaf admin** decides whether the leaf vouches for its users at
    all, since vouching shows their usernames to the hub.
  - **The hub admin** decides whether to accept vouched users, and from
    which leaves.
- **The hub admin sets what vouched users may do,** per feature: for
  example chat but not voice, or joining lobbies but not creating them.

## 7. Assets and avatars

### 7.1 Where assets live

| Asset | Where |
|---|---|
| Icons, 2D avatars, ANSI art | The user's AT repo |
| Small 3D avatars (glTF/VRM, a few MB) | The user's AT repo, if their PDS accepts the size |
| Large avatars, environments, leaf-specific art | Hosted by the leaf |
| Defaults for new users | The hub, or defaults a leaf supplies |

An asset reference is therefore either "blob in this AT repo" or "URL on this
leaf", and clients handle both.

AT blob limits (checked 2026-11-06): the protocol sets none; each PDS sets
its own and reports it (`blobUploadLimit` in `describeServer`). The reference
PDS default is now 100 MB. bsky.social's actual limit and any per-account
quota are unconfirmed, so large assets on users' Bluesky accounts should be
treated as a courtesy, not a right. Bluesky's own record types cap images at
1 MB; our record types set their own limits.

Assets are public once used: blobs referenced by records are publicly
readable, and assets shown to other people can't be protected anyway. The
hub fetches and caches them rather than having browsers load them from
users' PDSes, which the spec advises against.

### 7.2 Different avatars in different places

A user keeps a **wardrobe** of assets and a **default**. Each place decides
which one applies, so being one avatar on one leaf and another elsewhere, at
the same time, is no problem.

Per-leaf choices are **stored on the leaf, privately, not in the AT repo**.
Everything in an AT repo is public, so records like "my avatar for BBS X"
would reveal which leaves a person uses.

Resolution order on a leaf:

1. The avatar the user chose on that leaf (stored on the leaf)
2. An avatar the leaf hosts for that user
3. The user's AT default (our record type)
4. Their Bluesky profile picture
5. The hub's or leaf's default

The hub uses the same order, starting from the user's hub choice.

The at3d.app draft record types for 3D identity and VRM avatars (see the
[hub spec](https://github.com/zerosquaredio/nexus-lounge/blob/master/docs/hub.md)
§5.3) may cover the wardrobe; follow or help shape them rather than defining
our own where they fit.

### 7.3 Themes and ratings

Leaves, hubs and assets have **themes** (hacking, anime, politics, furry,
…): free, loose tags for finding and filtering. Separately, each has a
**rating** from a short fixed list (`general`, `mature`, `adult`), carried
as AT labels, because access depends on it. By default a hub lets every
published asset from every connected leaf be used, apart from keeping
adult content out of all-ages spaces. A hub can narrow that to keep to a
theme, and adult spaces admit only visitors with an age attestation the
hub accepts. Details: [hub spec](https://github.com/zerosquaredio/nexus-lounge/blob/master/docs/hub.md)
§5.4.

### 7.4 Avatar budgets

One person's heavy avatar can slow everyone else's client. So each space
sets an **avatar budget** (triangles, texture memory, and the like), and
avatars over it are shown to others as a simpler fallback; people can
also choose to hide heavy avatars themselves. VRChat's performance ranks
are the precedent. An avatar's cost is asset metadata, next to its
rating, and its lower levels of detail (§8.3) can serve as the fallback.

## 8. Common space and presence

- **Presence** (who is on which leaf, and where) is the core of the shared
  space. It is opt-in, like member activity elsewhere: off unless the person
  turns it on.
- Interaction depends on the leaves connected, each person's client tier,
  and the hub admin's settings. New users (with no account on any leaf) may
  be allowed in, if the hub admin chooses.
- **The hub works like a co-working space** (2026-11-08), at its simplest
  with no 3D at all:
  - **Text chat like IRC:** the hub is one big room by default, with
    channels, and direct or group sessions people start between
    themselves. Text goes through the hub; it's cheap.
  - **History is ephemeral by default:** a short scrollback, then gone.
    A channel can turn logging on, and then shows that it is logged.
    Nothing stops participants keeping their own logs, so "not logged"
    means the hub keeps no record, not that the chat is private.
  - **Voice like Zoom,** again one big room by default, with smaller
    rooms and calls. Small calls go direct between people where they can
    (§5.4). Past about five people, everyone sending to everyone stops
    working, so the big room uses a media server at the hub (an SFU such
    as LiveKit, Janus or mediasoup). A big room works better if people
    can gather into groups within it, which the views (§8.3) can show.
  - Software: an IRCv3 server (such as Ergo), or our own over WebSocket,
    fits the ephemeral model better than [Matrix](https://matrix.org),
    which keeps history and copies it between servers. Matrix's design
    is still a reference for direct messages and federation.
- 3D and VR: Overte, chosen 2026-11-08 (hub spec §5.3). Its domain
  servers map onto leaves or the hub's default space, and its metaverse
  server onto the hub directory, so the work is linking its accounts and
  directory to AT identities rather than building an engine. A browser
  and WebXR client, forked from Vircadia's web SDK, would join the same
  worlds. See the
  [Overte evaluation](https://github.com/zerosquaredio/nexus-lounge/blob/master/docs/overte_evaluation.md).

Ideas for the 3D stage (direction, not yet designed):

### 8.1 Building in the space

The space should have a built-in way to create and program things from
inside it: make an object, write the code that makes it behave, and share
it, without leaving the space. Overte already runs JavaScript scripts on
objects and in clients, which is a starting point (to be confirmed when
trying it). Scripts and the assets they drive would be ordinary assets
(§7), stored where their author chooses and shareable like any other.
Code from other people runs sandboxed, with permissions the space's owner
(hub or leaf admin) sets.

### 8.2 The network made visible

Hubs and leaves should be things you can see and touch in the space, like
the old cyberpunk pictures of the internet: each leaf an object you can
walk up to, read (its listing, status and hours), and enter (its terminal
or its own space), with the connections between them drawn as lines of
data, such as tunnels to the hub, peering between BBSes (networking spec),
and links between federated hubs (§10).

Most of this comes from public data that already exists: leaves' listings
and status, hubs' observed status and agreements, all AT records (§2.1),
plus live, aggregate figures from the hub (which leaves are connected, how
busy a link is). It never shows an individual's activity unless they have
turned presence on (§8). Because the data is public, anyone can build
their own view of it, in the space or outside it.

**Views are assets too.** A view (the code and models that turn this data
into a picture, whether a 2D map on a web page or a whole 3D landscape) is
made, shared, and traded like any other asset (§7, §8.1): people develop
their own, others pick the one they like or remix it, and a hub or leaf
can offer its own as the default. A simple 2D map on the hub's web page
could be the first one, once leaves publish listings and the hub
publishes observed status.

### 8.3 Views and the live model

Once there is a space (anyone present, in 3D or not), the hub publishes a
**live model** of it: who is present, groups (who is talking together, or
standing together in 3D), links between people and things, leaves and
their connections, and positions where there are any. A view is a
renderer of that model. The same model can be shown as:

- **ANSI:** groups of people arranged by who is talking to whom, or by
  who is near whom in 3D. It could run as a door on any BBS, so a caller
  on an ordinary terminal can see and talk to the hub.
- **Detailed 2D:** icons and graphics for people, leaves and the data
  between them (§8.2).
- **3D in a browser** (WebXR).
- **Overte,** and within it, simpler or more detailed renderings of the
  same space.

**Different hubs, the same views.** Hubs will want different things in
their model (teams, scores, traffic). If each hub defined its own, a view
would work only on the hubs it was written for. So, as glTF, IRCv3 and AT
lexicons do:

- **A small core model that every hub publishes,** defined by the
  framework and versioned (record types such as
  `nexus.cyberspace.space.*`): entities (people, leaves, rooms), presence,
  groups, links, and optional positions. Every view renders the core, so
  any view works on any hub, at least at a basic level.
- **Everything else is a named extension** (`org.somehub.teams`), and
  every extension object falls back to a core type: a team is a group, a
  server rack is a leaf, a quest marker is an entity with a label and
  icon. A view that doesn't know an extension still draws something
  sensible. Unknown fields are ignored, never an error.
- **A manifest:** the hub lists its core version and extensions; a view
  lists the extensions it uses and the ones it requires. Clients show
  each view as working fully, working without something ("no team
  colors on this hub"), or not fitting. Most views should need nothing
  beyond the core.
- **The model says what is there, not how it looks.** Layout, color and
  style belong to views. A hub that wants a say publishes optional hints
  ("team A is red") that views may follow or ignore.
- **Extensions that catch on become core.** Extensions are published as
  lexicons, which AT can resolve, so anyone can look one up. Within a
  major version the core only grows, so existing views keep working.

Mismatches then cost detail, not breakage.

**Requirements and levels of detail.** Views vary widely, from a terminal
to Overte, and may need different client applications and hardware.
Rather than "needs a discrete GPU and 100 Mbps", which goes stale, a view
or asset describes **what it uses**:

- the **runtime** it needs (terminal, browser, WebGL2 or WebGPU, WebXR,
  Overte): the one hard requirement;
- its **download size** and **steady bandwidth**;
- a **rendering budget** (triangles, texture memory, draw calls).

The client knows its own hardware and connection, compares, and says
"should run well", "may be choppy" or "can't run". The figures stay true
as hardware changes; only the verdicts change. Where it can, a view or
asset also ships **levels of detail**: lower-detail models and compressed
formats (glTF already has these: meshopt or Draco meshes, KTX2 textures),
as streaming video offers several bitrates. Each client picks the level
it can handle and drops down when it struggles, so a slow machine gets a
coarser picture rather than a broken one. People still find what suits
them mostly by trying views. Optionally, clients could report how a view
ran (opt-in, anonymous), so its listing can say "runs well on most
machines like yours".

**Slow clients don't hold back the space.** The hub never waits for its
slowest client:

- The hub holds the state, and sends each client only what is near it,
  at a rate that client can take; stale position updates are dropped or
  merged, since only the latest matters. A slow client falls behind
  alone.
- Voice through the media server gives each listener a quality that
  suits their connection; a listener's bad line affects only them.
- Laggy avatars are smoothed and shown as poorly connected (dimmed, for
  example) rather than jumping about.
- Avatar budgets (§7.4) stop one heavy avatar slowing everyone else.
- Games that keep all players in lockstep, where the slowest sets the
  pace, run as their own sessions (§5.4, §9), not in the common space.

Much of this may already be done by Overte and by WebXR libraries
(interest management, levels of detail, avatar limits). Check what they
provide when trying them, use theirs, and add only what's missing,
chiefly the declared requirements, which views need whatever renders
them.

### 8.4 Personal views and changes to the space

Views can carry their own assets and models, and code: to recolor
people, give them assigned models instead of their own (their own are the
default), change the environment, or analyze what's happening, such as
traffic patterns. Two kinds, with different permissions:

- **Personal:** changes only what you see (simpler models, your own
  colors, an overlay). Needs no one's permission, since no one else is
  affected.
- **Shared:** changes what everyone sees (team colors, the environment).
  Needs the permission of the space's owner, the hub or leaf admin, as
  for building (§8.1).

Overte appears to make the same split (client scripts and entity
scripts; to be confirmed).

Rules for view code:

- Sandboxed, reading only what the viewer could already see; no network
  access beyond what it declares; published as an asset under its
  author's identity, with a rating like any asset (§7.3).
- **Who talks to whom is sensitive.** Traffic and grouping views follow
  the presence rule (§8): opt-in, shown only to people who could see that
  presence anyway, and aggregated where possible.

### 8.5 Agents

Agents could change the environment or models on someone's behalf.

- An agent acts **as its user**, on the user's own model account, or on
  a free model; the hub doesn't pay for it.
- It has only the permissions given for its task, and what it does is
  marked as done by an agent. Shared changes need the user's
  confirmation.
- **Prompt injection is the main risk:** an agent reading the chat or the
  space can be steered by whatever others put there. So no agent holds
  more power than its one task needs.

## 9. Games

The BBS world already has a precedent: door networks such as DoorParty and
BBSlink (both supported by ENiGMA½) let callers from different boards play
the same door games. A hub is the modern, generalized version.

### 9.1 Kinds of game

| Kind | Runs on | Players need | Example |
|---|---|---|---|
| **Door games** | The leaf | Nothing; it's in the terminal | LORD, multi-node doors |
| **Hosted servers** | The leaf, or a server it names | A client, reaching the server through the route table (relayed or direct) | A Doom or Quake server the BBS runs |
| **Peer sessions** | One player's machine | The client, plus a session network (§5.4) | Two users' Doom match over an overlay LAN |

### 9.2 Catalogue and lobby

- **Catalogue (fairly static):** each leaf publishes its games as AT records:
  name, description, platform, kind, session sizes (one-to-one, up to N,
  hub-public), network needs, and how to get the client. As public records,
  the hub or any directory can list games across all leaves.
- **Lobby (live):** open sessions with host, game, free slots, and
  visibility (invite-only, the leaf's members, everyone on the hub, later
  across federated hubs). It changes by the second, so it is a live hub
  service built on the same machinery as presence, not AT records (which are
  permanent and slow to propagate). Leaves report their own sessions to it.

### 9.3 Downloads

Distributing executables is the classic way to spread malware, so:

- **Pin and sign:** a catalogue entry carries the download's content hash,
  signed by the leaf's DID; clients refuse a download that doesn't match.
- **Link to official sources** (the game's own site or release page) rather
  than rehosting, where possible.
- **Licensing:** leaves list only games that are freely redistributable or
  that players obtain themselves (Doom's shareware data and Freedoom are
  redistributable; the commercial game isn't).
- **Prefer sandboxed formats:** [js-dos](https://js-dos.com) runs DOS games
  in the browser with no install, and its multiplayer networking runs over
  WebSocket, which the hub can relay; WebAssembly builds of open games work
  the same way. Many classic games then become "click to join", even for
  browser-only users.
- **Hub policy** decides whether its lobby lists native downloads at all, or
  only door, browser, and hosted games.

Door games plus browser games in a hub lobby are a stage 2 target, well
before any VR.

## 10. Hub federation

Hubs can register with each other to form a larger space, or let their users
visit each other's leaves. IRC shows the risk: one shared state across a tree
of servers splits when a link breaks (netsplits). Two lighter models avoid
that:

- **Each room is owned by one hub**, and other hubs relay into it. A broken
  link isolates only that link; nothing needs merging afterwards.
- **Replicated rooms built to heal after splits**, as in Matrix.

For the directory, AT itself does most of the work: each hub's listings are
records, and other hubs subscribe to them.

## 11. Open questions

- ~~**Record type names beyond BBSes.**~~ Decided: `nexus.cyberspace.*`,
  grouped by role (§2.2). Links and vouched identities (§6.3, §6.4) are not
  records.
- Which tunnel software also carries real-time UDP traffic, or whether that
  needs a separate relay (TURN, QUIC).
- Exact offer, request, and agreement fields; how usage summaries are
  published.
- Rules for multi-homed leaves (directory duplicates, which hub's rules apply).
- Whether presence, the lobby, and common-space features are part of the hub
  or a separate service beside it.
- Federation model for common spaces (§10).
- How handoff tickets are issued and checked (§5.3), and the format of
  leaf-vouched statements (§6.4); possibly the same mechanism.
