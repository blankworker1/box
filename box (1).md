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

### The mechanism
- Each locker in use for Bitcoin custody holds **one steel seed plate**, engraved with a BIP39 seed — a single, independent wallet, not a sub-account of any shared/parent wallet.
- The keybar carries an attached **key fob with a printed QR code encoding the wallet's xpub** (extended public key) — watch-only, cannot spend, cannot risk the private key.

### Dual verification
1. **Point of transaction (mechanical)**: the magnetic key wand physically opens the correct locker. Proves access, nothing about contents.
2. **Point of opening (cryptographic, read-only)**: the xpub QR lets anyone — before *or* after physical access — independently verify the wallet's balance via their own wallet software, with zero risk to the private key. This can be shared digitally, ahead of a trade, as part of a pre-trade agreement — the Bitcoin equivalent of sending an invoice before a deal is agreed, something no purely physical bearer instrument (cash, gold) can offer.

### Locker as "ledger entry" — the analogy, precisely
- A locker = one UTXO container, not an account: it holds one specific, fixed-amount bearer instrument until swept.
- Opening + taking the plate = withdrawal; the on-chain settlement (sweeping funds elsewhere) happens after physical custody changes hands, on the recipient's own device — the locker itself never touches the blockchain and is not a "node" in the networking sense.
- Depositing = sealing a new plate inside; the "ledger entry" is the physical, sealed, occupied locker — legible to nobody but whoever holds the key.
- This is an analogy, not a literal ledger: nothing proves a locker's claimed contents match reality except opening it, the same trust model as any bearer cash.

### Critical rule: one wallet per locker
An xpub exposes visibility into every address its wallet has ever used or will use — not just one balance. If multiple lockers' seed plates were sub-accounts of one master wallet for convenience, a single leaked xpub would expose the whole matrix's activity to watch-only surveillance. **Each locker's wallet must be independent**, mirroring the "no shared database" rule already applied to the box electronics.

### The double-spend problem — and its fix
The original depositor always retains knowledge of the seed the instant it's written, regardless of lock quality, padlock genuineness, or physical settlement. No mechanism at the BOX layer can close this gap — it is not a BOX problem, it's the general Bitcoin custody-handoff problem, and it has a standard, zero-mechanism fix:

**The recipient sweeps to their own wallet immediately on taking possession.** Once swept, the original depositor's knowledge of the old seed becomes worthless, the same way any emptied wallet's seed is worthless after its coins have moved. This is a protocol rule — a habit — not a hardware feature, and it's the correct way to close this gap without adding complexity.

### What "risk-free" verification does and doesn't cover
The xpub gives risk-free **balance** auditing. It does not protect against the seed having been **copied before it was ever locked away** — a steel plate is engraved information, and information is photographable. Physical settlement proves nobody else can *open the locker* afterward; it does not prove the original depositor didn't retain a copy of the seed. This is the same limit every bearer seed instrument carries (see [[coin-tainer]]) — Bitcoin custody is defined by knowledge of the seed, not possession of any one physical object bearing it. At family-and-friends scale this rests on the same trust already underwriting the whole MVP.

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

- First physical site for the MVP mockup (La Prova truck flush-mount vs. a fixed Criccieth/Bosa location)
- Source and confirm single-key (no spare) magnetic padlock units
- Decide stamped-serial vs. NTAG424 for padlock genuineness marking, and at what point the upgrade is warranted
- Finalize the pre-trade message format (xpub QR + lock serial + any other agreed terms)
- Sequence for eventually surfacing the Bitcoin-custody use case publicly, if at all
- Detailed pairing/reset interaction flow for the Phase 2 electronic tier
- Hasp/staple grade selection for the MVP, given it's the weakest point independent of lock choice
