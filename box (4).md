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

### The tagline as protocol, not slogan
Each word in **BOX. SWAP. STORE. SETTLE.** maps to a specific moment in the mechanism:
- **SWAP** — the barter/goods layer: family-and-friends trading between each other.
- **STORE** — the locker at rest, holding contents (goods or a seed plate) between transactions.
- **SETTLE** — the padlock/keybar handoff at completion of a transaction: physically irreversible the instant it happens, the same finality Bitcoin settlement has on-chain.

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
- **16 unique setups required before launch**: 16 padlock/keybar pairs (single-key sourced) and 16 unique 5a seed plates engraved and fixed to their doors — a concrete pre-launch checklist, not just a design principle.

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

## 5. Layer 3 — Bitcoin (seed plate + QR keyfob)

Two distinct things, not a maturity ladder — 5a doesn't graduate into 5b, they're different in kind and coexist permanently:

- **5a is BOX-native infrastructure**: a fixed, locker-owned plate providing a simple, always-available transfer method for low-value amounts — built into the locker itself, the same way the hasp is. Anyone using that locker for this purpose is using a *service BOX provides*, with nothing personal to bring, own, or carry away except a passphrase.
- **5b is a personal bearer object**: an independently seeded plate that belongs to a person, exactly like the padlock/keybar. BOX is merely the venue where it can be stored or settled — the locker holds it, but the wallet's identity and security are entirely the owner's, carried away with them, not tied to that locker or any other.

Same relationship as Layer 2's padlock-and-hasp split: the locker is dumb, neutral infrastructure either way; what varies is whether the *thing inside* belongs to the infrastructure (5a) or to the person passing through it (5b).

### 5a. BOX-native low-value transfer — the fixed plate as infrastructure

**Every locker, no exceptions.** A unique, permanently engraved 12-word BIP39 seed plate is fixed to the inside face of every locker door at build time — bolted or welded on, not something that's added, removed, or made optional per locker. No locker is distinguishable from any other by whether it has one; all of them do. There's no decision to make at deployment about which lockers get this capability, and no visible difference — opening any door in the matrix reveals the same fixture. This is the 5a-equivalent of "one box, one key": every locker is uniformly, silently Bitcoin-capable from day one, indistinguishable from any locker that's only ever used for a phone charger.

Each plate's 12-word seed is reused indefinitely across many successive occupants and transactions. The 12 words are treated as **public, not secret** — anyone who has ever opened that locker has seen them, and the design doesn't fight that. All security instead lives in a **BIP39 passphrase** (the "25th word"), chosen fresh for each transaction, which combines with the fixed 12 words to derive a wallet with no cryptographic relationship to any other passphrase's wallet on the same plate.

**Flow:**
1. Depositor reads the plate's 12 words, chooses a new passphrase, and derives a fresh wallet.
2. Depositor deposits funds into that wallet, agrees the transaction, and hands over the keybar.
3. The passphrase is sent to the new holder separately (chat, email, or similar) — never with the keybar.
4. The depositor also shares the derived wallet's **own xpub** (computed once the wallet exists), so the new holder can verify the balance before travelling — restoring the same risk-free, pre-commitment verification the independent-seed design gives, without needing physical access to derive anything.
5. New holder visits the locker, opens it with the keybar, reads the 12 words (already public), applies the passphrase they were sent, and derives the same wallet.
6. **New holder immediately sweeps to their own separate personal wallet.**

**Why this works — a 2-of-2 split across two independent channels:**
Nothing is spendable from either channel alone. The keybar (physical, locker-gated) proves access to the plate; the passphrase (sent digitally, separately) proves the right to derive that session's wallet. An attacker needs both the keybar *and* the separate message to do anything — a real two-factor handoff, not a single shared secret.

**Hard rule — a derived wallet is burned the instant it's opened:**
The depositor read the 12 words and chose the passphrase, so they always retain full knowledge to re-derive and re-sweep that exact wallet, independent of the locker or plate. The recipient sweeping to a separate personal wallet immediately on derivation is not optional — it's the entire security boundary of this tier. Treat the derived wallet as burned whether or not funds are found.

**Passphrase transmission deserves real care, not casual habit:**
Since the passphrase now carries all the cryptographic weight, send it the way you'd send a password-reset code, not a throwaway aside — plaintext chat/email sits in logs and syncs to other devices by default. Splitting delivery (part verbal, part digital) or using a disappearing-message channel is worth the small extra friction.

**New risk from the "always attended, in public" MVP setting: shoulder-surfing at the point of use.** Reading a plate's 12 words, typing a passphrase, or scanning an xpub QR in front of a truck, an attendant, or passers-by is a real observation risk that didn't exist in an unattended-kiosk framing. Nothing about the mechanism changes to fix this — it's a practice, not a hardware gap — but it's worth naming as a live constraint on how the ritual is actually performed: doing the passphrase-entry/wallet-derivation step on a phone held low and angled away, choosing a quiet moment rather than mid-performance, and treating the locker's interior as a private space even though the truck itself is public.

**Scope**: a permanent BOX service for low-value amounts among the trust group — not a phase to outgrow. Not suitable for larger amounts or for anyone outside the trust group, since the 12 words being public-by-design means the passphrase alone stands between the plate and the funds; larger amounts belong in 5b instead.

### 5b. Personal bearer object — independent seed plate, owned by the individual

- The plate itself is a **personal bearer object**, engraved with its owner's own unique BIP39 seed — a single, independent wallet, never a sub-account of any shared/parent wallet, and never reused across transactions or owners. It belongs to whoever holds it, the same way a padlock does; a locker is just wherever it currently sits.
- Its keybar carries a **key fob with a printed QR code encoding the wallet's xpub** — watch-only, cannot spend, cannot risk the private key. Shareable digitally ahead of a trade, the same pre-commitment verification property as 5a's derived-wallet xpub.

**Critical rule: one wallet per plate.** An xpub exposes every address its wallet has used or will use, not just one balance. Sharing a parent wallet across plates would let one leaked xpub expose activity across every locker that plate ever passes through — the same "no shared database" principle already applied to the box electronics, extended here.

### Locker as "ledger entry" — the analogy, precisely (applies to both)
- A locker = one UTXO container, not an account: it holds one specific, fixed-amount bearer instrument until swept.
- Opening + taking the plate/deriving the wallet = withdrawal; on-chain settlement happens after physical custody changes hands, on the recipient's own device — the locker itself never touches the blockchain and is not a "node" in the networking sense.
- Depositing = deriving a fresh passphrase-wallet on BOX's own fixed plate (5a), or sealing a personally-owned plate inside for storage/settlement (5b) — the "ledger entry" is the physical, sealed, occupied locker, legible to nobody but whoever holds the key.
- This is an analogy, not a literal ledger: nothing proves a locker's claimed contents match reality except opening it, the same trust model as any bearer cash.

### The double-spend problem — and its fix (applies to both)
Whoever created the wallet — BOX itself via the fixed plate's passphrase (5a), or the plate's owner at engraving time (5b) — always retains knowledge of the underlying secret, regardless of lock quality, padlock genuineness, or physical settlement. No mechanism at the BOX layer can close this gap — it's the general Bitcoin custody-handoff problem, and it has a standard, zero-mechanism fix:

**The recipient sweeps to their own separate wallet immediately on taking possession.** Once swept, the prior holder's knowledge becomes worthless, the same way any emptied wallet's seed is worthless after its coins have moved. This is a protocol rule — a habit — not a hardware feature.

### What "risk-free" verification does and doesn't cover
The xpub gives risk-free **balance** auditing in both cases. It does not protect against the underlying secret (5a's passphrase, 5b's seed) having been **copied before or during use** — physical settlement proves nobody else can *open the locker* afterward, not that no copy was retained. This is the same limit every bearer seed instrument carries (see [[coin-tainer]]) — Bitcoin custody is defined by knowledge of the secret, not possession of any one physical object. At family-and-friends scale this rests on the same trust already underwriting the whole MVP.

---

## 6. What Stays Deliberately Out of Scope (MVP)

- **Courier/remote delivery** (sender not physically present at handoff) — deferred as a separate, later protocol layered on top, solved by in-person intermediary handoff, never by modifying the mechanism itself.
- **Any electronics at Layer 1/2** — the MVP hasp-and-padlock system is intentionally non-electronic, non-networked, and non-cryptographic beyond the xpub itself.
- **Cross-locker status/availability signalling** — no app, no notifications, no "box is free" messaging requiring any backend. If ever added, it must remain local-only (a box's own status shown on its own door) — never cloud-based, and never reveal *what* a box holds, only whether it's in use.

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
- Finalize the pre-trade message format (xpub QR + lock serial + any other agreed terms)
- Sequence for eventually surfacing the Bitcoin-custody use case publicly, if at all
- Detailed pairing/reset interaction flow for the Phase 2 electronic tier
- Hasp/staple grade selection given truck-mounting (vibration, movement) rather than a static wall
- Preferred passphrase-transmission channel for 5a transfers (split verbal/digital vs. disappearing-message app)
- Where the practical line sits between "low-value, use 5a" and "significant enough, bring your own 5b plate"
- Fixing method for mounting each 5a plate to its door (bolted vs. welded) and how it survives the door's working life without being disturbed
- Truck's power source for 16-compartment phone charging (existing mains hookup, battery, or solar) and load sizing against it
- Physical siting of the 16-locker panel on the truck (which side, height, reach) so the whole matrix is comfortably usable during a performance
