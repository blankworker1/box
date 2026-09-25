# BOX

![BOX logo](./box-logo.jpg)

*A self-sovereign, community barter layer, disguised as everyday storage.*

**BOX. SWAP. STORE. SETTLE.**

---

## 1. Concept

BOX is a matrix of identical, individually secured storage compartments that operates on two readings at once:

- **Publicly**: ordinary self-service storage — phone charging, small personal storage, left-luggage — legible at a glance, needing no explanation.
- **Privately**: a peer-to-peer barter layer, indistinguishable from the outside. No compartment reveals what kind of thing it holds — goods, cash, or a self-minted Bitcoin coin passing through like anything else (see [[coin-tainer]] Type S, a separate, independent protocol that BOX can host but doesn't depend on).

### Design principles
- **One box, one key.** No PINs, no secondary factors, no override credential.
- **No intermediary.** No staff, no operator-in-the-loop for normal use.
- **No cloud.** No shared database, no network dependency.
- **No other services.** Each part of the system does one job.
- **Trust the box, not us.** BOX names what it is: a simple tool, a simple mechanism, no intermediary required to operate.

### What kind of infrastructure this is
BOX is not trying to be trustless infrastructure for strangers. It's local infrastructure for a high-trust community — closer in spirit to a village lock-up than to an exchange or a custodian. Every layer pairs a real trust assumption (who you hand a key to, who you trade with) with real engineering sized to that assumption — enough to mitigate the actual attack surface, not built to survive adversarial strangers it was never meant to serve. That boundary is deliberate: extending BOX to people with no existing relationship to the group is a real redesign, not a scale-up, and several of the open questions below exist precisely because they'd need revisiting first.

### The tagline as protocol, not slogan
Each word in **BOX. SWAP. STORE. SETTLE.** names an ethos, paired with a physical reality that makes it concrete — two sequential acts on one object, since a barter good only ever does one job at a time.

- **STORE** — *the ethos of saving.* The reality: a jar of honey, sealed in a locker, doing nothing, worth exactly what it was worth yesterday.
- **SWAP** — *the ethos of barter.* The reality: the same jar, in the same hand, now the collector's — nothing about the honey changed, only who's holding it.
- **SETTLE** — *the ethos of finality.* The reality: the padlock/keybar handoff at the moment a trade completes — physically irreversible the instant it happens. The old holder's key is gone, not copied, not shareable; there's no way back in.

### MVP framing: "family and friends"
The first build is deliberately informal, tested among people who already trust each other and each other's judgement — not strangers. This bootstraps the mechanism where trust is already present, before it ever needs to operate where trust is absent.

**Concrete MVP spec**: a **4×4 matrix (16 lockers)**, mounted on the La Prova truck, with phone charging fitted throughout. No signage, no explanation, no labelling distinguishing this from any ordinary charging/storage point. **Always attended in public** — a person is present whenever the truck operates, but purely as passive presence (part of the truck/performance context), never as an operator: nobody holds a master key, nobody grants or mediates access, nobody's role touches the padlock/keybar mechanism at all. "Attended" describes the setting, not a role in the protocol — the "no intermediary" principle holds unchanged; a person being nearby is not the same as a person being in the loop.

---

## 2. The Two-Layer Model

Each layer has a different owner and a different trust model. A weakness in one layer does not propagate into the other.

| Layer | What it is | Who owns it | What it proves |
|---|---|---|---|
| **Boxes** | Hasps on a matrix of identical compartments | The site/infrastructure | Nothing — pure neutral geography |
| **Users** | Padlock + magnetic keybar | Whoever currently secures that hasp | Physical access right — who can open *this* locker, right now |

No single layer, alone, gives an attacker (or an outside observer) the full picture. A third, independent layer — a self-minted Bitcoin coin — can pass through either locker or hand, but belongs to neither; see [[coin-tainer]] Type S.

---

## 3. Layer 1 — Boxes (the hasps)

The locker bank itself carries **no lock hardware** in the MVP — just bare steel hasps mounted to a wall, cabinet, or the side of a vehicle (e.g. the La Prova truck). This is deliberately dumb, neutral infrastructure: any padlock can be clipped to any hasp.

### Sizing note
Ryanair-cabin-bag dimensions (55×40×20cm, +5cm clearance) are the reference size for the eventual full-scale build. MVP test units can be smaller (bench-scale, using an off-the-shelf charging locker or mini safe as a stand-in shell) — proving the mechanism and ritual matters more than matching final dimensions at this stage.

### Testing the layout cheaply
- A plywood/offcut mockup with real hasps fitted, at true or near-true scale, tests the *spatial* layout (grid feel, door size, opening/closing action) for the cost of hardware-store fittings.
- Populate the mockup bank with a mix of ordinary items, tested with family/friends — proves the concept experientially: does the ritual feel identical regardless of contents.
- This is fully decoupled from testing any electronics — the idea and the hardware can be validated on separate timelines.

### MVP configuration — 4×4, truck-mounted, phone charging
- **16 lockers**, identical footprint, arranged 4×4 on the La Prova truck's side panel.
- **Phone charging fitted to every compartment** as the public-facing function — the same role a Ryanair-bag left-luggage frame or parcel point played in earlier versions of the design, but charging reads as an even more ordinary, explanation-free everyday act.
- **Power source**: needs deciding against whatever the truck already runs (a mains hookup at performance sites, or the truck's own battery/solar, per any existing on-board power system) — charging load across 16 compartments should be sized against that before committing to a wiring plan.
- **Mounting durability**: hasps and their fixings need to tolerate a truck's vibration and movement over time, not just static wall-mount loads — worth checking fixing choice against that specifically, not assuming a wall-mount spec carries over unchanged.
- **16 unique setups required before launch**: 16 padlock/keybar pairs (single-key sourced) — the whole pre-launch checklist.

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
Padlock and keybar are established as a matched pair at handover:
- **Single-key sourcing**: explicitly order/select single-key units, or make destroying any spare key a visible ceremony step — most commercial padlocks ship with two keys as standard, which would silently undermine the "unique bearer object" property if not addressed.
- **Genuineness mark**: a stamped serial number on the padlock body lets a recipient visually check the lock they're told to expect against the one they find. Sufficient for the family-and-friends trust tier; an NTAG424-tagged padlock (reusing the same cryptographic approach as the box electronics) is the upgrade path if this ever needs to defend against deliberate forgery by people who don't already trust each other.
- **Reuse is intended**: the padlock-and-keybar pair is meant to circulate as a portable bearer "coin" — usable on any hasp, carried away by whoever holds it after opening, not fixed to one locker.

---

## 5. What Stays Deliberately Out of Scope (MVP)

- **Courier/remote delivery** (sender not physically present at handoff) — deferred as a separate, later protocol layered on top, solved by in-person intermediary handoff, never by modifying the mechanism itself.
- **Any electronics at Layer 1/2** — the MVP hasp-and-padlock system is intentionally non-electronic, non-networked, non-cryptographic.
- **Cross-locker status/availability signalling** — no app, no notifications, no "box is free" messaging requiring any backend. If ever added, it must remain local-only (a box's own status shown on its own door) — never cloud-based, and never reveal *what* a box holds, only whether it's in use.
- **Bitcoin-specific mechanics of any kind.** BOX carries no wallet logic, no seed handling, no settlement tooling. A self-minted coin can sit in a BOX locker the same way a jar of honey can — see [[coin-tainer]] Type S for that entire layer, developed and maintained independently.

---

## 6. Phase 2 — The Electronic Tier (future)

A separate, higher tier for boxes that warrant the added assurance of cryptographic access control (screen, local one-time codes, no operator override at all):

- **Credential**: NTAG424 DNA NFC tag per box — SUN messaging (AES-128 challenge-response, replay-proof), trust-on-first-use pairing, no PIN, no shared database.
- **Controller**: ESP32 (favoured over Pi Zero for this tier — lower power, faster boot, right-sized for read-tag/flip-relay/update-screen tasks), driving a PN532/MFRC522 reader and a relay-driven 12V solenoid strike.
- **Display**: local-only OLED/e-ink, showing box status and a locally generated one-time code — never transmitted over any network.
- **Physical module**: 30×50×60cm door units grouped as 6-box (3×2) cassette modules, ~94×102×60cm footprint, mounted via a flange plate bridging container-wall corrugation.
- **Container build**: 20ft shipping container, single wall (36 boxes) expandable to both long walls (72 boxes) via a pre-run power backbone from day one. Off-grid solar (roof PV) → charge controller → battery → inverter → per-module 240V→5V/12V PSU, with per-module breaker isolation.
- **Threat model**: no override, ever — a lost tag or dead controller means the box is genuinely unrecoverable, an accepted cost of real bearer custody at this tier.

This tier remains a later build target, not a prerequisite for the family-and-friends MVP, which is designed to validate the concept, the ritual, and the two-layer trust model using nothing but hasps and padlocks.

---

## 7. Open Questions

- Source and confirm single-key (no spare) magnetic padlock units, ×16
- Decide stamped-serial vs. NTAG424 for padlock genuineness marking, and at what point the upgrade is warranted
- Detailed pairing/reset interaction flow for the Phase 2 electronic tier
- Hasp/staple grade selection given truck-mounting (vibration, movement) rather than a static wall
- Truck's power source for 16-compartment phone charging (existing mains hookup, battery, or solar) and load sizing against it
- Physical siting of the 16-locker panel on the truck (which side, height, reach) so the whole matrix is comfortably usable during a performance
