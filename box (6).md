# BOX

![BOX logo](./box-logo.jpg)

*A self-sovereign, community layer for barter and Bitcoin, disguised as everyday storage.*

**BOX. SWAP. STORE. SETTLE.**

---

## 1. Concept

BOX is a matrix of identical, individually secured storage compartments that operates on two readings at once:

- **Publicly**: ordinary self-service storage — phone charging, small personal storage, left-luggage — legible at a glance, needing no explanation.
- **Privately**: a peer-to-peer barter and Bitcoin bearer-custody layer, indistinguishable from the outside. No compartment reveals what kind of thing it holds.

### Design principles
- **One box, one key.** No PINs, no secondary factors, no override credential.
- **No intermediary.** No staff, no operator-in-the-loop for normal use.
- **No cloud.** No shared database, no network dependency.
- **No other services.** Each part of the system does one job.
- **Trust the box, not us.** BOX names what it is: a simple tool, a simple mechanism, no intermediary required to operate.

### What kind of infrastructure this is
BOX is not trying to be trustless infrastructure for strangers. It's local infrastructure for a high-trust community — closer in spirit to a village lock-up or a community savings bank than to an exchange or a custodian. Every layer pairs a real trust assumption (who you hand a key to, who you trade with, who mints a coin) with real engineering sized to that assumption — enough to mitigate the actual attack surface, not built to survive adversarial strangers it was never meant to serve. That boundary is deliberate: extending BOX to people with no existing relationship to the group is a real redesign, not a scale-up, and several of the open questions below exist precisely because they'd need revisiting first.

### The tagline as protocol, not slogan
Each word in **BOX. SWAP. STORE. SETTLE.** names an ethos, paired with a physical reality that makes it concrete. STORE and SWAP are two sequential acts on one object — a barter good only ever does one job at a time. SETTLE can't take that same simple shape, because Bitcoin, inside BOX, has to do two of money's classical jobs at once rather than one after another — so it splits into two distinct realities rather than one.

- **STORE** — *the ethos of saving.* The reality: a jar of honey, sealed in a locker, doing nothing, worth exactly what it was worth yesterday.
- **SWAP** — *the ethos of barter.* The reality: the same jar, in the same hand, now the collector's — nothing about the honey changed, only who's holding it.
- **SETTLE** — *the ethos of money as both a store of value and a means of exchange at once.* The reality: a sealed, marbled coin passed hand to hand, its balance rising with each visit to the workshop, resting for months or years between moves, never opened unless its owner chooses to — value that persists precisely because nobody touches it. Self-minted, individually owned, carried rather than deposited.

*(An earlier fast-settlement mechanism, 5a, would have given SETTLE a second reality — instant, walk-up, no ceremony. It's deferred; see §5 and §6.)*

Money classically claims a third job too — unit of account, a fixed denomination. BOX deliberately refuses it. No coin carries a printed value; nothing is ever fixed at mint, only ever attributed, continuously, by whoever trusts it enough to add to it. That's not an omission — printing a number would mean claiming a fixed truth the whole architecture is built specifically not to claim.

### MVP framing: "family and friends"
The first build is deliberately informal, tested among people who already trust each other and each other's judgement — not strangers. This bootstraps the mechanism where trust is already present, before it ever needs to operate where trust is absent. Barter/gift and Bitcoin custody use cases are tested side by side, with the Bitcoin layer present but not publicly announced at this stage (a deferred reveal, consistent with the whole project's camouflage principle).

**Concrete MVP spec**: a **4×4 matrix (16 lockers)**, mounted on the La Prova truck, with phone charging fitted throughout. No signage, no explanation, no labelling distinguishing this from any ordinary charging/storage point. **Always attended in public** — a person is present whenever the truck operates, but purely as passive presence (part of the truck/performance context), never as an operator: nobody holds a master key, nobody grants or mediates access, nobody's role touches the padlock/keybar/plate mechanism at all. "Attended" describes the setting, not a role in the protocol — the "no intermediary" principle holds unchanged; a person being nearby is not the same as a person being in the loop.

---

## 2. The Three-Layer Model

Each layer has a different owner and a different trust model. A weakness in one layer does not propagate into the others.

| Layer | What it is | Who owns it | What it proves |
|---|---|---|---|
| **Boxes** | Hasps on a matrix of identical compartments | The site/infrastructure | Nothing — pure neutral geography |
| **Users** | Padlock + magnetic keybar | Whoever currently secures that hasp | Physical access right — who can open *this* locker, right now |
| **Bitcoin** | Steel seed plate + QR keyfob (xpub) | The value itself | What's actually inside — verifiable independent of who controls the lock |

No single layer, alone, gives an attacker (or an outside observer) the full picture.

---

## 3. Layer 1 — Boxes (the hasps)

The locker bank itself carries **no lock hardware** in the MVP — just bare steel hasps mounted to a wall, cabinet, or the side of a vehicle (e.g. the La Prova truck). This is deliberately dumb, neutral infrastructure: any padlock can be clipped to any hasp.

### Sizing note
Ryanair-cabin-bag dimensions (55×40×20cm, +5cm clearance) are the reference size for the eventual full-scale build. MVP test units can be smaller (bench-scale, using an off-the-shelf charging locker or mini safe as a stand-in shell) — proving the mechanism and ritual matters more than matching final dimensions at this stage.

### Testing the layout cheaply
- A plywood/offcut mockup with real hasps fitted, at true or near-true scale, tests the *spatial* layout (grid feel, door size, opening/closing action) for the cost of hardware-store fittings.
- Populate the mockup bank with a mix of ordinary items and steel seed-plate props, tested with family/friends — proves the dual-reading concept experientially: does the ritual feel identical regardless of contents.
- This is fully decoupled from testing any electronics — the idea and the hardware can be validated on separate timelines.

### MVP configuration — 4×4, truck-mounted, phone charging
- **16 lockers**, identical footprint, arranged 4×4 on the La Prova truck's side panel.
- **Phone charging fitted to every compartment** as the public-facing function — the same role a Ryanair-bag left-luggage frame or parcel point played in earlier versions of the design, but charging reads as an even more ordinary, explanation-free everyday act.
- **Power source**: needs deciding against whatever the truck already runs (a mains hookup at performance sites, or the truck's own battery/solar, per any existing on-board power system) — charging load across 16 compartments should be sized against that before committing to a wiring plan.
- **Mounting durability**: hasps and their fixings need to tolerate a truck's vibration and movement over time, not just static wall-mount loads — worth checking fixing choice against that specifically, not assuming a wall-mount spec carries over unchanged.
- **16 unique setups required before launch**: 16 padlock/keybar pairs (single-key sourced) — a concrete pre-launch checklist, not just a design principle. Bitcoin capability (5b) is carried by whoever owns a self-minted coin, not fixed into the lockers themselves.

---

## 4. Layer 2 — Users (padlock + keybar)

### The mechanism
A **magnetic keyed padlock** (e.g. Squire CP-class or Madol-style magnetic-key units, "40mm" class) — fully mechanical, no battery, no electronics. The magnetic key wand is a **bearer object**: exclusive physical possession, not a shareable secret like a shared PIN or combination code. Handing over the key wand is a genuine, exclusive transfer — the previous holder retains nothing.

### Why this beats a shared-code mechanism
A mechanical combination lock (tested as an early zero-hardware rehearsal) has a structural flaw: the person who set the code still knows it after sending it to someone else, and can reopen the box in the gap before the new holder resets it. A physical magnetic key wand doesn't have this problem — once handed over, it's genuinely gone from the sender's possession.

### Honest security limits (vs. the electronic/NTAG424 tier)
- Vulnerable in principle to a strong external magnet worked against the lock with force — a known characteristic of the magnetic-key category, not cryptographically hard like NTAG424 SUN messaging.
- The hasp/staple is itself a weak point independent of lock quality (bolt croppers, prying) — hasp grade matters as much as lock choice.
- No cryptographic exclusivity — a magnet pattern can in principle be physically characterized and duplicated by someone with the right tools.

### Setup ceremony
Padlock and keybar are established as a matched pair at the same ceremony as seed-plate engraving:
- **Single-key sourcing**: explicitly order/select single-key units, or make destroying any spare key a visible ceremony step — most commercial padlocks ship with two keys as standard, which would silently undermine the "unique bearer object" property if not addressed.
- **Genuineness mark**: a stamped serial number on the padlock body, noted in the same pre-trade message as the xpub (see Layer 3), lets a recipient visually check the lock they're told to expect against the one they find. Sufficient for the family-and-friends trust tier; an NTAG424-tagged padlock (reusing the same cryptographic approach as the box electronics) is the upgrade path if this ever needs to defend against deliberate forgery by people who don't already trust each other.
- **Reuse is intended**: the padlock-and-keybar pair is meant to circulate as a portable bearer "coin" — usable on any hasp, carried away by whoever holds it after opening, not fixed to one locker.

---

## 5. Layer 3 — Bitcoin (the self-minted coin)

BOX's Bitcoin layer is now a single mechanism: the self-minted, sealed coin (formerly "5b"). A second, faster mechanism — a locker-owned plate offering instant, no-ceremony transfers, referred to below as **5a** — was designed and prototyped, but is deliberately **deferred**; see §6 for why, and the condensed record kept below for if it's ever revisited.

### The coin — a self-minted, sealed single-sig object

A physical implementation of [[coin-tainer]]'s existing philosophy, using a BIP39 plate instead of an NTAG424 chip as the credential. Not a demo tier — this is the true bearer-object standard, the physical peer to a Casascius coin.

**The object:**
- A steel plate, engraved with a single, independently-generated 12-word BIP39 seed — one wallet, never a sub-account, never reused across owners.
- Fully encased in **injection-molded, multi-color waste plastic**. The turbulent mixing of recycled material at injection produces a genuinely unique, non-repeating marbled pattern per unit — real physical entropy, not decorative uniqueness, and it uses waste material in keeping with the ecological grounding running through the rest of this work.
- The wallet's **xpub is printed on the outside** as a QR — watch-only, cannot spend, visible without breaking the seal.

**What the seal actually does — a physical property, not a cryptographic one:**
Bitcoin has no concept of a deposit-only address; anyone holding the 12 words can always spend. "Value can only be added, not withdrawn" holds only because withdrawal requires the seed, and the seed is physically inaccessible while the casing is intact. Full encasement makes this genuinely destructive to defeat — no seam to lift, no coating to peel — unlike a dip or hologram coating, which can in principle be removed and reapplied. The owner retains the right to break their own seal and sweep the funds to their own wallet at any time; the seal is a convenience and a ritual, never a restriction on the legitimate holder's sovereignty.

**The self-mint rule — learned from Casascius's one real failure:**
Casascius coins used the same public-address / hidden-key shape, and the only thing that ever undermined trust in them was that they were minted centrally — for a moment, the minter held every key, and trust rested on believing they'd destroyed their copies. BOX avoids this by requiring the eventual owner to generate the entropy, engrave the plate, and seal it themselves, in one uninterrupted session, with nobody else present. **No pre-minting a batch of sealed coins for convenience** — every coin is minted only at the moment someone takes possession of it. This mirrors the same self-sovereign generation principle already used in [[sd-card-ceremony]], applied to a plate instead of a microSD card.

**The provenance gallery — what it actually protects, and how it's hosted:**
Each mint (photo of the finished coin + its xpub) is published as a note to the truck's own local Nostr relay — the same TAZ Tools infrastructure already built for [[zona-permuta-taz]], reached over the truck's unbranded wifi zone, no website, no remote hosting, nothing BOX operates beyond what already exists. This keeps the gallery inside the same physical/network boundary as the trade itself: the wifi zone's range, already the zone's defining perimeter, doubles as where a coin can be checked against its record. Records can ride the same one-directional outward sync to the GitHub Node that TAZ already uses, for durability beyond any single session.

The gallery does **not** prevent theft by someone who already has physical possession of a coin — nothing can, once the casing is broken. What it defends against is **fraud in a later resale**: a broken-and-resealed or counterfeit coin can't be passed off as genuine and still-sealed, because a buyer can compare the physical object to its permanently published photo, and the marbling pattern is effectively impossible to reproduce.

**A free verification byproduct of the xpub being public:** whether a coin has ever been opened and swept doesn't need the gallery to track status at all — it's answerable on-chain by anyone, at any time: no outgoing transaction ever, presumably still sealed; any outgoing transaction, it's been redeemed, whether by a thief or by the rightful owner exercising their own right to break the seal. The gallery only ever needs to record the original mint, once, permanently.

**Critical rule: one wallet per plate.** An xpub exposes every address its wallet has used or will use, not just one balance — already the intended and accepted trade-off here, since the xpub is deliberately public from the moment of minting. What must never happen is a *shared* parent wallet across multiple plates, which would let one coin's xpub expose activity across every other plate derived from it — the same "no shared database" principle already applied to the box electronics, extended here.

### Locker as "ledger entry" — the analogy, precisely
- A locker = one UTXO container, not an account: it holds one specific, fixed-amount bearer instrument until swept.
- Opening + taking the plate = withdrawal; on-chain settlement happens after physical custody changes hands, on the recipient's own device — the locker itself never touches the blockchain and is not a "node" in the networking sense.
- Depositing = sealing a personally-owned, self-minted plate inside for storage or settlement — the "ledger entry" is the physical, sealed, occupied locker, legible to nobody but whoever holds the key.
- This is an analogy, not a literal ledger: nothing proves a locker's claimed contents match reality except opening it, the same trust model as any bearer cash.

### The double-spend problem — and its fix
Whoever created the wallet always retains knowledge of the underlying secret, regardless of lock quality, padlock genuineness, or physical settlement. No mechanism at the BOX layer can close this gap entirely — it's the general Bitcoin custody-handoff problem. The coin is meant to circulate sealed, not be swept on every handoff, so its protection is the **self-mint rule**: if the ceremony is followed correctly, nobody but the current owner ever knew the seed in the first place, so there's no prior holder's knowledge to worry about until the owner themselves chooses to pass the coin on. The risk shifts entirely to whether that rule was actually followed at minting.

### What "risk-free" verification does and doesn't cover
The xpub gives risk-free **balance** auditing. It does not protect against the seed having been **copied before or during minting** — physical settlement proves nobody else can *open the locker* afterward, not that no copy was retained. This is the same limit every bearer seed instrument carries (see [[coin-tainer]]) — Bitcoin custody is defined by knowledge of the secret, not possession of any one physical object. At family-and-friends scale this rests on the same trust already underwriting the whole MVP.

---

## 6. What Stays Deliberately Out of Scope (MVP)

- **Courier/remote delivery** (sender not physically present at handoff) — deferred as a separate, later protocol layered on top, solved by in-person intermediary handoff, never by modifying the mechanism itself.
- **Any electronics at Layer 1/2** — the MVP hasp-and-padlock system is intentionally non-electronic, non-networked, and non-cryptographic beyond the xpub itself.
- **Cross-locker status/availability signalling** — no app, no notifications, no "box is free" messaging requiring any backend. If ever added, it must remain local-only (a box's own status shown on its own door) — never cloud-based, and never reveal *what* a box holds, only whether it's in use.
- **5a — the fast-settlement mechanism, deferred.**

### Why 5a is deferred

5a was a fixed, locker-owned plate offering instant, no-ceremony Bitcoin transfers: scan the locker's public 12 words, generate a fresh private half and passphrase via a dedicated tool (**the SETTLE App**), hand off screen-to-screen, sweep immediately. It was the one genuinely novel piece of engineering in BOX — a public plate minting unlimited unrelated wallets is a pattern no existing scheme (BIP85, SLIP39, standard passphrase wallets) does — and it filled two real gaps: a fast, sats-sized *conguaglio* (the top-up that settles an uneven barter trade, same word already used for permuta) without needing a whole self-minted coin for a trivial amount, and a zero-ceremony on-ramp for someone not yet ready for the self-mint ceremony.

It's deferred for reasons of coherence, not capability. Everything BOX has converged on — self-mint, mastery, the workshop, [[anyone-can-make-this-money|Same Rules, Different Product]] — describes patient, local, craft-based money. 5a was the opposite on every axis: instant, walk-up, no ceremony, available to someone who'd never set foot in the workshop. Keeping it would have meant BOX's own Bitcoin layer working against the values the rest of the project argues for. Removing it makes BOX, cleanly, a Slow Money implementation with nothing inside it pulling the other way.

**Condensed record, for if this is ever revisited:**
- Three data blocks — Locker (public, fixed), Generated (private half + computed 24th word), Passphrase (fresh per transaction) — each traveling on a different channel, none alone sufficient.
- A working prototype exists: real BIP39 entropy and SHA-256 checksum via native Web Crypto, QR-scan-driven UX on both the depositor and recipient sides, destination address scanned from the recipient's own wallet rather than typed. It compiles and displays the completed phrase for external-wallet import; it does not yet perform EC derivation, signing, or broadcast for true one-tap auto-sweep.
- Security rested on immediate-sweep discipline (a habit, not a hardware property) and a documented in-person/remote mode distinction, with remote mode an explicit, named trade-off rather than a silent default.
- If revisited, the open question is whether it can be reconciled with the Slow Money framing at all, or whether it's better kept as a permanently separate, explicitly-labeled "fast lane" rather than folded back into BOX's main identity.

---

## 7. Phase 2 — The Electronic Tier (future)

A separate, higher tier for boxes that warrant the added assurance of cryptographic bearer custody (screen, local one-time codes, no operator override at all):

- **Credential**: NTAG424 DNA NFC tag per box — SUN messaging (AES-128 challenge-response, replay-proof), trust-on-first-use pairing, no PIN, no shared database.
- **Controller**: ESP32 (favoured over Pi Zero for this tier — lower power, faster boot, right-sized for read-tag/flip-relay/update-screen tasks), driving a PN532/MFRC522 reader and a relay-driven 12V solenoid strike.
- **Display**: local-only OLED/e-ink, showing box status and a locally generated one-time code — never transmitted over any network.
- **Physical module**: 30×50×60cm door units grouped as 6-box (3×2) cassette modules, ~94×102×60cm footprint, mounted via a flange plate bridging container-wall corrugation.
- **Container build**: 20ft shipping container, single wall (36 boxes) expandable to both long walls (72 boxes) via a pre-run power backbone from day one. Off-grid solar (roof PV) → charge controller → battery → inverter → per-module 240V→5V/12V PSU, with per-module breaker isolation.
- **Threat model**: no override, ever — a lost tag or dead controller means the box is genuinely unrecoverable, an accepted cost of real bearer custody at this tier.

This tier remains a later build target, not a prerequisite for the family-and-friends MVP, which is designed to validate the concept, the ritual, and the three-layer trust model using nothing but hasps, padlocks, steel plates, and paper/printed QR codes.

---

## 8. Open Questions

- Source and confirm single-key (no spare) magnetic padlock units, ×16
- Decide stamped-serial vs. NTAG424 for padlock genuineness marking, and at what point the upgrade is warranted
- Sequence for eventually surfacing the Bitcoin-custody use case publicly, if at all
- Detailed pairing/reset interaction flow for the Phase 2 electronic tier
- Hasp/staple grade selection given truck-mounting (vibration, movement) rather than a static wall
- Truck's power source for 16-compartment phone charging (existing mains hookup, battery, or solar) and load sizing against it
- Physical siting of the 16-locker panel on the truck (which side, height, reach) so the whole matrix is comfortably usable during a performance
- Source the injection-molding process and multi-color waste plastic feedstock for the coin's casing; prototype and stress-test tamper-evidence before trusting it with real value
- Define the Nostr note format for a mint record (photo + xpub) and confirm it rides the existing TAZ Tools one-directional sync to the GitHub Node
- Whether the truck's local relay persists across sessions or resets per Prova, and what that means for gallery durability if the outward sync isn't yet built
- Write the self-mint ceremony script itself (script/checklist for the moment of generating, engraving, and sealing a coin, one owner alone), including the practice-phase/learner's-permit step from [[anyone-can-make-this-money]]
- What (if anything) fills the *conguaglio*-shaped gap 5a was covering — a fast, small-value top-up for an uneven barter trade — without reintroducing 5a itself
- What serves as a lighter on-ramp for someone not yet ready for the self-mint ceremony, now that 5a's zero-ceremony path is gone
