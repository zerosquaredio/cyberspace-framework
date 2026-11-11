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
| `nexus.cyberspace.space.*` | Nowhere: not records | The shape of a hub's live model (§8.3), defined as Lexicon schemas for a hub's live stream and queries, so it stays out of AT repos (§2.1) |
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

### 3.1 How settled each part is

This document mixes decisions with ideas. As of 2026-11-08:

| Part | Status |
|---|---|
| Identity: leaves and users as AT accounts (§6.1 to §6.4) | **Decided**, and built for BBSes |
| Record type names (§2.2) | **Decided** |
| Stage 1: directory and tunnel | **Planned in detail** (Nexus Lounge hub plan) |
| The 3D software: Overte (§8) | **Decided** as a choice; untested (§11) |
| Offers and agreements, sizing (§4) | Direction |
| Transport policy, routes, handoff, connections between users (§5) | Direction |
| Assets, avatars, themes and ratings (§7.1 to §7.3) | Direction; rating rules drafted in the hub spec |
| The hub as a co-working space; chat and voice (§8) | Direction |
| The live model and views (§8.3) | Direction |
| Games (§9) | Direction |
| Where presence comes from (§8.6) | **Decided** |
| What comes next (§3.2) | **Decided** |
| Blocking and moderation (§8.7) | Direction |
| Signing in once, by hub ticket (§6.5) | Direction; wanted with stage 4, not needed for BBSes in a browser |
| Avatar budgets, building in the space, the visible network, personal views, agents (§7.4, §8.1, §8.2, §8.4, §8.5) | Ideas |
| Objects, styles and shared work: backends, authority, restyling, trust groups, shared media, contributor awards (§8.8) | Ideas |
| Hub federation (§10) | Idea |

### 3.2 What comes next, and what is parked

*Decided 2026-11-08.* After stage 1, the next thing built is a small
first slice of stage 2, not 3D: it is far cheaper, everyone can use it
whatever their client, and it is what makes a hub a place rather than a
list.

**The first slice of the common space:**

1. People sign in to the hub with AT, and get a hub profile (§6.3).
2. Presence, from the hub's own client and from leaves reporting it,
   including members without an AT account where the leaf and the hub
   allow it (§8.6). This needs a small BBS feature.
3. Text chat: one room, ephemeral (§8).
4. The core live model, as a stream (§8.3).
5. Two views of it: a 2D map on the hub's web page, and an ANSI view
   that runs as a door on a BBS.
6. The basics of blocking (§8.7): muting someone for yourself, the hub
   admin blocking someone, and a leaf admin keeping a member out.

In the plans these are Nexus Lounge hub features H13 to H18 and BBS
features F18 and F19.

Done when two people using different BBSes see each other on the map and
chat, one from a browser and one from inside a BBS terminal.

**After that,** roughly in order: voice; assets and avatars (stage 3);
3D with Overte (stage 4), and with it tickets for signing in once
(§6.5), which the 3D clients and non-BBS leaves are the first to need.

**Any time:** the Overte experiments (run a domain, try the client, test
the three untested assumptions, §11). They are cheap, and they decide
whether stage 4 is practical as designed.

**Parked:** written down so the design leaves room for them, but not
planned until the stages above exist:

- overlay networks for game sessions (§5.4), and native game downloads
  (§9.3);
- hub federation (§10) and multi-homed leaves;
- agents (§8.5) and building in the space (§8.1);
- objects with their own backends and rules, style sheets and
  restyling, trust groups, shared media, and contributor awards (§8.8);
- a browser and WebXR client for Overte worlds;
- paid, ID and age verification (so adult spaces wait too; adult leaves
  can still be listed, by rating);
- a hub-run PDS for leaf and user accounts;
- published sizing figures and the load-test tool (§4.4);
- BBS clusters (Nexus Lounge networking spec).

## 4. Hubs and leaves

### 4.1 Offers, requests, and agreements

The relationship between a hub and a leaf is a handshake, published as AT
records so anyone can check it:

| Record | In whose repo | Says |
|---|---|---|
| **Offer** | Hub | What the hub relays, the services it runs for people (§8: text chat, voice, which kinds of view, 3D), bandwidth limits, theme or community, rules, terms (free, a suggested contribution, or a price) |
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

### 4.4 Sizing a hub

Someone setting up a hub needs to know what each service (§8) will cost
them for the number of people they expect. Over time the network itself
can answer that:

- **Hubs measure themselves.** The software already meters usage (§4.2);
  it also records, per service (text, voice, relay, 3D, assets):
  bandwidth per active user, peak people at once, CPU and memory, and
  monthly totals.
- **Hubs may publish those figures** as an opt-in stats record in the
  hub's AT repo, like a leaf's stats record (publishing spec §3.5).
  Anyone can collect them from every hub that publishes, with no central
  service, and turn them into a calculator: "100 people, text and voice,
  no 3D: about this much bandwidth, this size of server".
- **Usage is shared, not money.** Prices vary by provider and change
  often, and admins needn't reveal their bills. The calculator multiplies
  shared usage by current prices for the provider being considered.
- **Privacy:** only totals, rounded, per service, and only from hubs
  above a minimum size, since on a tiny hub the figures could reveal what
  particular people did.
- **Before there is real data,** a load-test tool connects simulated
  users (chatting, talking, moving) to a test hub to give first figures
  per service, and catches regressions as the software changes. Real
  figures gradually replace them.
- **On a running hub,** the admin's dashboard projects from its own
  figures ("at this growth, you'll pass your bandwidth allowance in about
  two months").

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

*Decided 2026-11-06; first built 2026-11-08 (Nexus Lounge F9).* This confirms the BBS account in Nexus Lounge's
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
  authenticate as the BBS's DID). This arrives in stages: a tunnel token
  first, then the leaf's DID, then agreements (Nexus Lounge hub plan,
  H3).
- **Hub-issued handles and addresses are conveniences, not identity.** A
  leaf moving hubs keeps its DID; its handle can change.

What follows from it:

- **Migration:** a new agreement with the new hub, the leaf's directory entry
  updated, clients following the new routes. The old hub keeps a forwarding
  note for a grace period, like an HTTP 301. Users are bound to DIDs too, so
  their accounts on the leaf don't depend on the hub.
- **The leaf's own account says where it is now.** Its listing record
  carries its current addresses under a DID that doesn't change, and a
  post tells its followers, who follow the DID, not the handle, so they
  stay even if a hub-issued handle is lost. This is the one channel
  neither hub controls: it still works when the old hub won't forward
  (a leaf that was dropped, or left on bad terms), and it works the
  same for a leaf with no hub at all.
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
| **AT account, no leaf** | Sign in to the hub, browse the directory, connect to leaves, apply on leaves that take new users. The common space only if the hub admin lets such people in (§8.7) |
| **AT account, member of a connected leaf** | Full hub membership: one identity across all leaves, avatars from their own repo, presence, the common space, everything across leaves |

There is **no separate hub registration**: logging in to the hub with AT is
the hub account. The hub keeps a small profile keyed by DID, created at
first login, and stores no passwords. Taking part in the common space
is a further step: by default it is for members of the hub's leaves
(§8.7).

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

### 6.5 Signing in once

*Direction, 2026-11-08; not needed for BBSes in a browser, and not built
until something needs it.* The hub and every leaf are separate AT login
clients, so a person approves at their account host once for the hub and
once for each leaf they open.

**For BBSes in a browser that is fine.** It is one approval per board,
the first time; after that the board's own web sign-in logs them in with
one click, and SSH callers have their saved key.

**It becomes a real need for:**

- **Clients with no convenient browser:** a VR headset or desktop app
  opening a leaf from inside the space, or moving from one BBS to another
  from inside a terminal.
- **Leaves that aren't BBSes:** a full AT login means publishing client
  details, holding keys and receiving callbacks. Reasonable for a BBS;
  heavy for a small game server, a chat room or an Overte world. Checking
  a signed ticket is a few lines of code, so for them it is how they
  learn who someone is at all. (Overte already works this way: a world
  trusts its accounts server's word.)
- **Handoff to a direct address** (§5.3), where the leaf must know who
  is arriving without a fresh login.

**For those: a leaf may accept a ticket from a hub it trusts, as one
more login type.**

- The person is signed in to the hub. When they open a leaf from the
  hub, the hub gives their client a ticket: their DID, the leaf's DID as
  the audience, an expiry a minute or so away, and a one-time number,
  signed with the hub's key (published in the hub's DID document).
- The leaf checks the signature, that the ticket is for itself, and that
  it hasn't been used. It then treats the DID as proven and continues
  exactly as after its own login: admission, account status and any
  two-factor rules still apply.
- **Off by default, and both sides opt in.** The leaf admin lists the
  hubs whose tickets the leaf accepts.
- **The cost is trust:** the leaf takes the hub's word for who someone
  is, so a dishonest or broken-into hub could enter as anyone on the
  leaves that trust it. So admins never log in by ticket unless the leaf
  admin says otherwise, and a leaf that has its own AT login can require
  it the first time a DID appears.
- A leaf with its own AT login keeps it, so it never depends on the hub.

Alternatives considered: the hub as the login client for all its leaves
(simplest for admins, but the hub sees every login and leaves can't work
without it); and separate logins only (today's behavior, and enough for
BBSes).

This is the handoff ticket of §5.3, and the mirror of a leaf-vouched
statement (§6.4): there the leaf vouches for a person to the hub, here
the hub vouches to the leaf. One signed-statement format should serve
all three.

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
as AT labels, because access depends on it. A leaf's theme can also
be a style sheet that restyles its area of the space (§8.8). By default
a hub lets every
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
  and the hub admin's settings. By default the common space is for
  members of the hub's leaves; people with no account on any leaf are
  let in only if the hub admin chooses (§8.7).
- **Hubs choose which services they run** (2026-11-08). One hub may offer
  text chat only; another text and ANSI or 2D views; another text and
  voice but no 3D; another only a VR space, with its own voice and no
  separate chat. Each is a legitimate hub, and the choice is partly cost:
  text is cheap, while voice rooms, relays and 3D need bandwidth and
  servers. The services are listed in the hub's offer (§4.1) and its
  manifest (§8.3), so:
  - leaves and people can browse hubs by what they offer;
  - clients show only the views and features the hub supports;
  - a view needs the services it relies on as well as the model
    extensions (a 3D view on a text-only hub doesn't fit).

  The software is the same for every hub (§4.3); these are switches, not
  forks. Federated hubs (§10) may offer different services, and what
  crosses between them is what both support.
- **The hub works like a co-working space** (2026-11-08), at its simplest
  with no 3D at all:
  - **Text chat like IRC:** the hub is one big room by default, with
    channels, and direct or group sessions people start between
    themselves. Text goes through the hub; it's cheap.
  - **History is ephemeral by default:** a short scrollback, then gone.
    A channel can turn logging on, and then shows that it is logged.
    Nothing stops participants keeping their own logs, so "not logged"
    means the hub keeps no record, not that the chat is private.
  - **Moderation without logs:** the hub keeps a short rolling buffer
    (minutes, in memory) so a report can carry what was just said, and
    a reporter's client can attach what it saw. Without that, an abuse
    report in an unlogged room has no evidence.
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
(hub or leaf admin) sets. Where an object's code runs, and who pays for
it, is in §8.8.

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

Once there is a space (anyone present, in 3D or not), the hub serves a
**live model** of it, as a live stream from the hub, never as AT records
(§2.1): who is present, groups (who is talking together, or
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

- **A small core model that every hub serves,** defined by the
  framework and versioned (Lexicon schemas under
  `nexus.cyberspace.space.*`, §2.2): entities (people, leaves, rooms), presence,
  groups, links, and optional positions. Every view renders the core, so
  any view works on any hub, at least at a basic level.
- **Everything else is a named extension** (`org.somehub.teams`), and
  every extension object falls back to a core type: a team is a group, a
  server rack is a leaf, a quest marker is an entity with a label and
  icon. A view that doesn't know an extension still draws something
  sensible. Unknown fields are ignored, never an error.
- **A manifest:** the hub lists its core version, extensions and services
  (§8), as part of its offer record (§4.1), so there is one public
  statement of what a hub provides; a view lists the extensions and services it uses and the ones it
  requires. Even a text-only hub publishes the core (who is present,
  channels as groups), which is cheap and lets simple views work. Clients show
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

### 8.6 Where presence comes from

*Decided 2026-11-08.* The hub relays a terminal session without knowing
who is in it (§6.3): the person proves who they are to the leaf, not to
the hub. So presence needs a source, and the hub never guesses it from
traffic. There are two, and **both are part of the first slice** (§3.2),
so that people who connect by SSH or at a leaf's own address aren't
missing from the space:

1. **The hub's own client.** Someone signed in to the hub who opens a
   leaf from the hub's page or app: the hub knows which leaf they opened
   and when that connection ends. Needs nothing from the leaf, so it
   works with any BBS software.
2. **The leaf reports it.** The leaf tells the hub "this person is
   here", over its connection to the hub, signed as the leaf. This covers
   every kind of session, and can carry finer detail the person chooses
   to show (in the doors, in a chat room). It needs a leaf-side feature,
   off by default, which the leaf admin turns on.

**People without an AT account can be reported too,** if the leaf admin
chooses: the report then names a local user rather than a DID, and the
hub shows them as `alice@coolbbs`. This is the leaf-vouched identity of
§6.4, in the same signed format, and as there the hub admin chooses
whether to accept them, from which leaves, and what they may do.

**A leaf's word is only a leaf's word.** A leaf could claim that any
DID is on it, and speak as them. So whatever arrives through a leaf is
shown as coming through it (`alice via coolbbs`), and counts as that
AT account's own presence only once the person, signed in to the hub
themselves, has confirmed that leaf, once.

Consent applies to both sources, and the stricter setting wins: the leaf
admin turns reporting on, each member then chooses whether they are
shown, and a person signed in to the hub has their hub setting too. The
rating rule also applies: presence on a leaf is shown only in spaces
rated at or above it (hub spec §5.4).

**Later:** letting someone with no AT account into the common space
itself, not only shown in it. For example their leaf gives them a link
that opens the hub's page as `alice@coolbbs`, carrying a vouched
statement. What they may do there is the hub admin's choice (§6.4).

### 8.7 Blocking and moderation

*Direction, 2026-11-08.*

**Trust, and the freedom to leave.** None of what follows is airtight,
and it isn't meant to be. What keeps a hub healthy is the relationship
between its admin and its leaves' admins, and that either can end it: a
hub can drop a leaf that causes trouble, and a leaf can leave a hub it
disagrees with and join another, keeping its identity and its members
(§4.3, §6.1). The tools below make everyday problems easy to deal with;
the framework's real job is to keep leaving cheap for both sides. There
will always be cases nobody thought of, and those are settled between
people, not by the software.

Who moderates a common space is a social agreement, and it differs from
hub to hub, so it is configurable. Some powers exist under every
agreement:

- **Anyone can mute or hide someone for themselves.** It changes only
  what they see and hear, so it needs no one's permission (as personal
  views, §8.4). The first line of defence, and the cheapest.
- **The hub admin can block anyone from the common space:** an AT user
  by DID, which follows them whatever leaf they come through; a vouched
  user by name and leaf; or everyone a leaf vouches for. The hub admin
  always has the last word, since the hub carries the legal exposure
  (hub spec §8).
- **A leaf admin can keep their own members out of the common space.**
  For a member the leaf vouches for, that is complete: no vouching, no
  entry. For a member with an AT account, the leaf stops reporting them,
  but they can still sign in to the hub by themselves; the leaf can pass
  its block to the hub, which honors it according to the agreement
  below.

**Who may join at all.** By default, only members of a leaf connected
to the hub take part in the common space (decided 2026-11-08). So
everyone in it was let in by some leaf admin, who can be asked about
them, and a blocked person who makes a new AT account must first be
admitted by a leaf before they are back. It is not airtight (a leaf
with open admission is hardly a gate), so the hub admin can adjust it:

- **Who:** leaf members only (the default); anyone with an AT account;
  or, later, visitors with no account at all.
- **Which leaves count:** for example, not those whose admission is
  simply `open` (their listing says, publishing spec §3.1).
- **A waiting period:** a member of a leaf for at least so many days.
- **Leaf standing:** the hub sees which leaf each person came in
  through, so a leaf that keeps admitting troublemakers stops counting.

The hub learns that someone is a member from the leaf: a signed
statement, the same as a presence report (§8.6), made only for members
who have chosen to take part, and never published.

A block has a scope (one room, or the whole common space) and may be for
a time or permanent. Being banned from a leaf and being blocked from the
hub are separate, unless the agreement joins them.

**The moderation agreement** is set by the hub admin and stated in
plain words in the hub's offer (§4.1), so leaf admins know it before
joining. The software doesn't encode agreements; it provides the pieces
(moderators, each with the powers the hub admin gives them, over
everyone or only their own leaf's members), and a hub arranges them to
match what it has agreed. Three common starting points:

| Model | Who moderates the common space |
|---|---|
| **Hub-moderated** | The hub admin, and moderators they appoint. Leaf admins can report |
| **Shared** | Some or all leaf admins are moderators too, with the powers the hub admin gives them (mute, remove from a room, block for a time, block permanently). Their blocks of AT users apply across the hub |
| **Each leaf its own** | Each leaf admin is responsible for their own members and can act only on them. The hub admin handles the rest |

- Moderators are named by DID, and every action is recorded with who
  took it (the actions, not the chat, which stays unlogged, §8).
- Someone blocked is told whether the hub or a leaf did it, and how to
  ask for it to be reviewed.
- Block lists are private to the hub, never AT records.
- **Evasion:** new AT accounts are free, which is why membership of a
  leaf is the default gate (above). Beyond it, a hub may ask more of people
  before they join the common space (account age, or an attestation,
  with the verifiers leaves already use). For vouched users the leaf is
  accountable: a leaf whose users keep causing trouble loses vouching.
- Optionally, a hub can follow a shared block list, such as a Bluesky
  moderation list, and leaves and hubs can share theirs, each deciding
  whether to honor the other's (as between peered BBSes, Nexus Lounge
  networking spec §6).

### 8.8 Objects, styles and shared work

*Discussed 2026-11-10; ideas, not yet designed.* One principle runs
through all of it: **the hub coordinates and relays; heavy work runs
with whoever wants it, by consent.** The hub stays small and cheap, and
the people and leaves who want something expensive supply the computing
or bandwidth for it.

#### Objects: an interface and a backend

An object in the space has two halves:

- **The interface:** what appears in the world. A model, a small amount
  of state, a display, and the actions people can take on it.
- **The backend:** where its work runs. It sends state updates to the
  interface and receives actions.

This is the door-game pattern again: the door runs on the BBS and the
terminal only displays it; here the object is the terminal. A light
object (a chessboard) can keep both halves together. A heavy one (a
miner, a simulation, an AI agent, §8.5) has only its interface in the
world.

| Backend | How it connects | Who pays |
|---|---|---|
| Inside the hub (light objects only) | Sandboxed code with a strict CPU budget | Hub |
| The leaf it came from | Through the leaf's tunnel | Leaf |
| The owner's own computer | Their client, or a small companion program | Owner |
| Anywhere else (a cloud server, a home lab) | An endpoint the hub connects to | Owner |

To the hub, the last three are the same: a remote backend whose updates
it relays through the live model (§8.3), with a bandwidth and update-rate
limit per object. If the backend goes offline, the interface stays,
showing that it's offline.

**The hub meters what it runs rather than trusting declarations.**
Objects declare their needs ("light, runs anywhere" or "needs a remote
backend"), but code inside the hub is WebAssembly with an instruction
budget ("fuel"). An object over budget is paused and marked as needing a
remote backend. This matters for the hub's own survival: most VPS
providers ban crypto mining, so a hub that *runs* a miner risks its
server, while one that only *relays* a miner's display doesn't.

#### Who decides: authority

Whoever runs an object's rules is its **authority**: their copy decides
what counts. Each model suits different objects:

| Authority | How | Suits |
|---|---|---|
| **The hub** (default for public objects) | The hub runs the rules, sandboxed | Public spaces, strangers |
| **The leaf it came from** | Actions go to the leaf through its tunnel | A leaf's own objects; the leaf pays |
| **The person handling it** | Their device runs it; ownership passes to whoever picks it up | Physics, casual play |
| **Everyone checks** | Every device runs the same deterministic rules and rejects an invalid action; the hub only orders and relays actions | Turn-based games with no hidden information, such as chess |

For this to work, an object's rules ship as **portable code**: a
WebAssembly module with a small fixed interface (state and action in,
new state or refusal out), deterministic (no clock, no randomness of its
own, no network), so the same file runs on the hub, a leaf, or a
browser. An object declares which authorities it accepts and what it
needs ("turn-based" or "needs fast response"); the hub admin says which
it allows (for example "no foreign code in the hub"). The hub keeps the
latest state, so if the authority leaves, the object moves to another or
pauses rather than being lost. Moves can also be signed and kept as a
record, so a chess game can be replayed and checked by anyone.

Precedent: Overte's client and server entity scripts, and its
*simulation ownership* for physics; Croquet (now Multisynq), where every
client runs the same code and a relay only orders events; Mozilla Hubs
passing ownership of objects.

Cheating isn't a priority now; these models are chosen for cost and
fit, and the "everyone checks" model resists cheating anyway.

#### Visitors' devices

Code that runs on *other people's* devices (in the "everyone checks"
model, or a view's code, §8.4) gets a tight budget: drawing, animation,
checking rules. Anything heavier is refused. Running heavy work on
visitors' machines without asking is cryptojacking. Heavy work on a
person's own machine needs their explicit consent, like the camera
permission: "This object wants to use your processor: allow while I'm
here / always / never". By default only the object's owner is ever asked.

#### Styles: restyling the space

Each leaf, and the hub, can publish a **style sheet**, like CSS for the
space: a palette, materials, shaders, lighting, post-processing, and
which heavier transforms it wants. Together with the leaf's default
environment, it gives a leaf's area its theme (cyberpunk, anime, furry,
§7.3). Leaves bring their environments and assets to the hub, where
everyone can use them (subject to licences, below).

Restyling happens in three tiers, by cost:

| Tier | Examples | Where it runs | Cost |
|---|---|---|---|
| **1. Live** | Recolouring to a palette, cel shading, outlines, pixelation, fog, lighting, fur shaders | Each visitor's device, while drawing | Almost none; the hub sends the original plus the style sheet |
| **2. Made once, cached** | Turning a model into blocks, a lo-fi version, reshaping (big eyes), AI restyling | A backend: the leaf wanting the style, the owner, a trust group (below), or the hub if its admin allows | Once per asset and style; the result is stored under the source's hash plus the style's hash, so every hub and visitor reuses it |
| **3. Remixed by hand** | An artist remakes a model in their leaf's style | The artist | A new asset, linked to its original |

So "the hub filters assets" mostly means the *client* filters them, at
no cost to the hub; only tier 2 needs computing, and only once. A leaf
can make its styled versions of everyone's assets on its own hardware and
offer them back.

Styles **cascade**, as in CSS:

1. The hub's base style.
2. A leaf's area: walk into it, and its style applies to everything
   there, visitors' objects included (Overte's zone entities already set
   lighting, haze and sky per zone).
3. The object's own rules: parts marked "don't restyle", such as a logo,
   a sign's text, or a chess piece that must stay recognisable.
4. The viewer's own preferences, last: no pixelation, high contrast, no
   flashing. These are accessibility overrides, like a user style sheet.

**Avatars are restyled only by consent**: "adapt my avatar to the local
style: yes / colours only / no". An avatar is someone's identity.

A style sheet is a named extension with a fallback (§8.3): a client that
can't run shaders applies only the palette, or shows the original. The
format can build on glTF (with material variants) plus a small style
document of our own, published by the leaf as an AT record, like its
listing.

#### Licences and credit

- Copying or modifying another leaf's asset makes a derivative work, so
  assets carry licence information (CC-BY, CC-BY-NC, no derivatives,
  this hub only), and hubs respect it.
- Tier 1 restyling is display, like a browser's dark mode, and is
  probably fine for any asset. Tier 2 makes a new copy, so "no
  derivatives" rules it out. Tier 3 needs the licence to allow it.
- Every derived asset links to its source (an AT record reference): who
  made the original, and which style made this version. Credit follows
  automatically, and a leaf leaving a hub can take back its originals.
- A restyled asset keeps at least its source's rating (§7.3).

#### Trust groups: sharing computation

People can pool their computers for shared work: a dozen people running
one miner object together, a leaf's members making its tier-2 styled
assets, rendering, simulations, volunteer science. A trust group answers
one question: **may this code use my resources?** The sandbox applies to
everyone whatever the trust, so the group isn't about safety; and some
work checks itself (a mining share, a chess move), or can be checked by
giving the same piece to two members, as BOINC does.

- **The group is an AT record** published by its creator (members,
  coordinator, code). **Each member publishes their own acceptance
  record**; membership counts only when both agree, as with a listing
  linked to a hub.
- **Consent is to specific code:** its content hash, signed by its
  author's DID. A new version needs new consent, unless the member chose
  "accept updates from this author". So nobody can swap in other code.
- **Each member sets limits:** processor and GPU share, only when idle or
  plugged in (off by default on phones and headsets), hours, an expiry,
  and **no network access except to the group's coordinator**. Their own
  device enforces them; the records only express consent. Leaving is
  instant.
- **Trust doesn't pass along:** being in a group with someone doesn't
  mean trusting their other groups.
- **The coordinator** is the object's backend (above): it splits the
  work, hands out pieces, collects results and credits contributors. The
  object in the world shows the group: total work, who is contributing,
  and a join button that opens the consent prompt. Mining pools, BOINC
  and Folding@home are the precedent.
- **The risk is a botnet, not dishonest results.** Group code with free
  network access could attack other sites. Hence no network beyond the
  coordinator, through the hub; and the hub sees group traffic and sets
  its rules ("no compute groups", "no mining", bandwidth caps).
- **It needs our own client.** A browser can share work only while its
  tab is open (Web Workers, WebGPU); background work, GPU limits and
  enforcing consent properly need our client, or a companion program
  beside the browser.

(Bitcoin itself is a poor example in practice: specialised mining chips
have made CPU and GPU mining unprofitable. The pattern is what matters.)

#### Shared media: a film for the room

Someone wants to play a film for everyone. Four ways, cheapest first:

| Way | How | Hub bandwidth | For |
|---|---|---|---|
| **1. Everyone fetches, the hub keeps time** (default) | The hub sends a small playback state: which video, started at hub time T, playing or paused. Each client streams it from the source and follows the shared clock | Almost none | Anything everyone can reach: public videos, a leaf's own files, services each viewer has access to |
| **2. The hub relays** | One upload; the hub's SFU forwards it | Viewers × bitrate (30 × 5 Mbps = 150 Mbps) | Live screen sharing; a private file only one person has |
| **3. Viewers pass it on** | The source sends it once; viewers share pieces with each other (WebTorrent-style, as PeerTube does) | About one copy | Big rooms, scheduled showings |
| **4. Downloaded ahead** | Clients fetch the file (by hash) before a scheduled showing, then play it locally at the shared time | None at showtime | Film nights, events |

- **Syncing:** each client plays at *now minus the start time*; it
  corrects small drift by playing a few percent faster or slower, and
  jumps if more than about half a second out. Pause, seek, and who holds
  the remote are updates to the same small state.
- **Selective, like voice:** people far from the screen get a lower
  resolution, people elsewhere get nothing, and the sound comes from the
  screen's position, positioned on each device.
- **Passing it on (way 3) is shared bandwidth, opt-in** ("I'll pass it
  on, up to 5 Mbps"). It shows viewers' addresses to each other, so it is
  for trust groups or people who choose it; others get it from the hub or
  the source. A hub's offer might require it, or way 4, for showings over
  a certain size.
- **The law matters as much as bandwidth.** A hub relaying a film (way
  2) is distributing it, and carries the copyright risk. Viewers each
  streaming from a source they can already reach (way 1) is a shared
  clock, which is how watch-party services stay legal. So a hub's rules
  might allow relaying only screen sharing and content the uploader owns.

#### Appreciating contributors

BBSes always recognised contributors (upload ratios, "top uploaders"
bulletins; ENiGMA½ has achievements). The hub can too:

- **What's counted:** work units completed (counted by the coordinator,
  not the contributor); media pieces passed on (confirmed by the
  receivers); assets contributed and restyled, and how much they're used
  (from the credit links); a leaf's backends that others use, and their
  uptime. This is appreciation, not payment, so light checks are enough.
- **Friendly, not a race:** listing is opt-in, like presence, and
  "anonymous contributor" is allowed; totals are weekly or monthly, never
  live (a live board would show who is online); awards recognise variety
  (first contribution, a long streak, the most-used asset, a newcomer),
  and use levels rather than strict ranks, since raw totals mostly reward
  whoever has the biggest GPU.
- **Awards are AT records:** the hub's own account (hub plan H6)
  publishes an award naming the member's DID ("Contributor of the Month,
  November 2026"). It is signed by the hub, so it can't be forged, and
  the member chooses whether to show it (both sides agree). Awards travel
  with people to other hubs and leaves, and a BBS can show them beside its
  own achievements.
- **Leaves too:** members' contributions add up per leaf ("this leaf's
  members made 400 styled assets this month"), giving leaves a reason to
  encourage them.
- **Later, perhaps:** contributions counting toward a leaf's share of
  hub costs (§4.1, §4.2). Kept as appreciation for now: once
  contributions are worth money, people have reasons to game them, and
  there may be tax questions.

#### How practical, and in what order

*Assessed 2026-11-10.* Almost all of this builds on existing software
and needs no research; the parts differ mainly in effort.

| Part | Practical? |
|---|---|
| Interface and backend objects | Yes, moderate: the backend protocol, how a backend proves who it is, rate limits. Works in the 2D map and ANSI view too |
| Hub or leaf authority; "everyone checks" for turn-based games | Yes; chess rules in WebAssembly are small and deterministic |
| Physics with ownership hand-off | Built into Overte; hard in our own web views. Physics doesn't compute identically on different machines, so it uses ownership, never "everyone checks" |
| Fuel metering | Yes: Wasmtime has fuel built in; elsewhere (Node, browsers) by instrumenting code when it loads, or a worker that is stopped |
| Consent prompts | Yes; ordinary interface work |
| Live styles (tier 1) | Yes in web views (three.js shaders); harder in Overte, where styling everything in a zone probably needs client changes |
| Cached restyling (tier 2) | Simplifying, blocks and recolouring, yes (gltf-transform, meshoptimizer). Reshaping and AI restyling are immature, and automatic results often look poor; artists (tier 3) cover the gap |
| Licences, credit, trust group records, awards | Yes, easy (awards need the hub's own account, hub plan H6) |
| Coordinator and companion program | Practical (BOINC shows it), but the biggest single project, and the most security-sensitive: a native program running other people's code |
| Shared media, ways 1, 2 and 4 | Yes, easy; LiveKit already does screen sharing |
| Shared media, way 3 | Moderate: libraries exist (WebTorrent, p2p-media-loader), but firewalls between home connections make it patchy |

The real problems:

- **Two worlds means doing things twice.** Overte runs JavaScript on its
  own engine; web views would use three.js and WebAssembly. To avoid
  writing every object and style twice, keep **rules in WebAssembly** and
  **looks as data** (glTF and the style sheet), with a thin adapter per
  client. Whether Overte's scripting can run WebAssembly is untested
  (§11).
- **Films in a browser's 3D world.** Browsers won't draw protected video
  (DRM services) or other sites' embeds, such as YouTube, onto a 3D
  surface. In a web 3D view, only directly hosted video that allows it
  can be a screen in the world; anything else becomes a floating 2D
  panel. Overte's native client can show web pages on surfaces, so it
  does better.
- **Automatic restyling quality** is an artistic risk, not a technical
  one.
- **Effort:** all of this together is years of work for a small team, so
  the order matters more than feasibility.

Suggested order, mostly without 3D:

1. A chessboard on the 2D map and in the ANSI view, using "everyone
   checks": it proves the interface and backend split, portable
   WebAssembly rules, and the live model, cheaply.
2. An object with a BBS backend, such as a door game's status shown in
   the space.
3. Synced media (way 1) and screen sharing.
4. Contributor awards, once the hub has its own account.
5. In 3D: live styles in web views, then in Overte once the experiments
   answer the WebAssembly question.
6. Last: cached restyling, peer media sharing, trust groups and the
   companion program.

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
- Chat software: an IRCv3 server, or our own over WebSocket (§8).
- Moderation: the exact powers in each model, how a leaf's block is
  passed to the hub, and how reviews work (§8.7).
- How the live model is carried (WebSocket stream, as the directory's
  live updates, hub spec §5.2) and how its core is versioned (§8.3).
- How view code is sandboxed in each runtime, Overte's especially (§8.4).
- Objects (§8.8): the WebAssembly runtime and fuel budgets for code in
  the hub and on visitors' devices; how a remote backend connects and
  authenticates; how authority is handed over; the style sheet format;
  licence metadata on assets; the trust group and acceptance record
  types; how the consent prompt works in each client.
- Overte: whether its accounts server can be self-hosted, whether a
  relay can carry a home-based domain's traffic, and how far the old
  web SDK has drifted (Overte evaluation §4, §5); and whether its
  scripting can run WebAssembly, for portable object rules (§8.8). All
  untested.
