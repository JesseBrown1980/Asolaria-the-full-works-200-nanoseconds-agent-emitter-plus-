# Asolaria — THE FULL WORKS: the 200-nanosecond agent emitter (plus the multi-emitter)

The PID-signal **source** that feeds the omnidispatcher. The dispatcher only *routes* `FEDENV`
envelopes — it never generates the PID signals. This repo is the **emitter**: the ~200ns single-thread
unit, the full emit→loop→reduce cycle, and the later **multi-emitter** design that divides threads and
multiplies the service toward **≈ 1.16 trillion agents / second**.

## 1. The unit — the 200ns agent emitter (MEASURED)
The base emitter is the **revolver PID emitter**: `PIDChainRevolver.next()` emits a brown-hilbert /
`sha16(seed)` PID — a **"200ns-class spawn id"** (`drive-wave-cascade-pipeline-60D-2026-06-03.cjs:30`).
One thread emitting a PID every ~200ns ≈ **5,000,000 PID/s**.

### 1b. Provenance — ONE spawner ran the 100-BILLION in ~5.4 hours (OPERATOR-CANON)
This single ~200ns spawner IS the **trigger root** — `200ns SPAWNER → EXECUTOR + FILE-MANAGERS →
ROUTERS → PRISM → GC` — and it produced the **100-Billion-agent run in ~5.4 hours from ONE spawner**,
on the Node side, **BEFORE** the Rust 8-byte host and **BEFORE** the multi-spinner. Coherence:
100,000,000,000 ÷ 5.4 h (19,440 s) ≈ **5.14 M spawn/s**, matching the MEASURED single-thread ~5 M PID/s
above. Achievable on one thread because emission is **VirtualPointer-dominant = 0 OS processes** (no
per-agent fork/exec). The ~1.16T regime in §3 is the *later, post-8-byte-host, multi-spinner* figure —
NOT this one-spawner run.

## 2. THE FULL WORKS — one cycle (`asolaria-loop.mjs`, operator 2026-06-01)
```
revolver.next()                       # 1. PID emitter (~200ns)  ← the emitter
  → ProjectRoomRouter.planSwapTo      # 2. RENAME project folder = defeat same-name throttle = FREE
    → runFreeAgent (despawn old, spawn fresh in the unique room)
      → HOOKWALL.pass                 # 4. PID-stamp → SCORE/GNN → verdict → tamper-evident observation
        → planPrismRoute              # 5. PRISM: many rooms → reverse_gain GNN → 1 answer  (THE REDUCTION)
          → GCRuntime.emit            # 6. gulp every N, flow-not-pile
loop ×100k = drives-as-RAM throughput
```
Nothing reinvented — revolver, ProjectRoomRouter, GCRuntime are the proven modules composed into the
kernel route. HBP only. Step 5 (PRISM) is a **reduction**: many rooms collapse through a reverse-gain
GNN into one answer.

## 3. PLUS — the multi-emitter (NOT one signal, many) — OPERATOR-CANON
The later design does **not** run one 200ns emitter on one thread. It **divides threads into N
emitters** and **multiplies the service**, and the rename-before-load seam makes each replica **free**:

| multiply | from → to | effect |
|---|---|---|
| **spindles** | 24 → 100 → 1,000 → 10,000 (Node slice) | each runs its own emit→loop→PRISM |
| **rooms × sectors** | 10,000 rooms (Node slice) → **100,000+ rooms × 113 SECTORS** (113 = prime(30) = D30 COSIGN_TRIPLE) | the current/target addressing grid; rename-before-load makes each replica FREE |
| **spinners** | 1 → **MULTIPLE simultaneous 200ns spinners** | multiplying spinners multiplies the PRISM reductions |
| **emitters** | 1 → N parallel revolver emitters (divide threads) | N independent streams |
| **per-emit wave** | 1 PID → 1 spindle (1 main + 5 subagents), 1 BEHCS-1024 glyph @ 520:1 | one ~200ns emit materializes a whole subsystem |

→ operator-canon throughput **≈ 1.16 trillion agents / second**. (The ~200ns single-thread unit is
MEASURED in code; the 1.16T rate is OPERATOR-CANON — emitters × spindles × 100k×113 grid × wave/addressing
amortization; the fully-arrived running state is UNVERIFIED / operator-gated / E=0.)

> **Enabler (OPERATOR-CANON):** the ~1.16T regime is unlocked by the **Rust 8-byte host**, which removes
> the Node revolver's **`DEFAULT_THROTTLE = 50`** concurrent ceiling. On the Node side (this repo) one
> spawner is throttle-bounded — that is the ~5 M/s, one-spawner, 100-Billion-in-5.4 h figure of §1b. The
> **100k+ rooms × 113 SECTORS grid + multiple simultaneous spinners** is the *post-8-byte-host* target;
> the 10,000-room code below is the **migration-precondition slice**, not the ceiling.

## 4. How it feeds the dispatcher
```
emitter(s) ─ BEHCS-1024 brown-hilbert PID seed, ~200ns, E=0 ─► FEDENV envelope (target=pid:H…)
   ×N threads / ×(24→10k) spindles                                    │ bus :4947 / pid-inboxes
                                            omnidispatcher ── route to slot ──► worker_thread
                                                                                │ spawner-emit (~200ns)
                                                              room / kernel / spindle MATERIALIZES
```
Many emitters → many parallel dispatch lanes → many parallel PRISM reductions → the trillion-agent
regime. **The dispatcher is the router; the emitter is the source, and there are many.**

## Source (`emitter/`)
- `pid-chain-revolver.mjs` — the ~200ns revolver PID emitter (the unit)
- `asolaria-loop.mjs` — THE FULL WORKS cycle (emit→room→agent→HOOKWALL→PRISM→GC, ×100k)
- `hbp-emitter.mjs` / `port-address-emitter.mjs` — the FEDENV / port-address emitters
- `project-room-router.mjs` — rename-before-load + planPrismRoute (the free seam + the reduction)
- `drive-wave-cascade-pipeline-60D-2026-06-03.cjs` — the 60D wave cascade (the multiplied driver)

## Companions
- **Dispatcher:** `JesseBrown1980/omni-dispatcher` (`EMITTER.md` + the router it feeds)
- **Reductions framing:** `what-is-asolaria---how-do-we-get-reductions-in-everything`
  (`MULTI-EMITTER-SERVICE-MULTIPLICATION.md`)
- **Algorithm:** `Algorithms-of-Asolaria` (`SERVICE-MULTIPLICATION-ALGORITHM.md`)

> **Status / carve-out:** source + docs only — no keys/seeds/tokens, no HBP/HBI corpus, no
> PID-Registration-Office bytes. Gated / E=0 in its original homes: all emit is describe-only / no-fire;
> actual emission to the live bus is the separate operator-gated step. Secret-scanned clean.
