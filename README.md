# Asolaria — THE FULL WORKS: the 200-nanosecond agent emitter (plus the multi-emitter)

The PID-signal **source** that feeds the omnidispatcher. The dispatcher only *routes* `FEDENV`
envelopes — it never generates the PID signals. This repo is the **emitter**: the ~200ns single-thread
unit, the full emit→loop→reduce cycle, and the later **multi-emitter** design that divides threads and
multiplies the service toward **≈ 1.16 trillion agents / second**.

## 1. The unit — the 200ns agent emitter (MEASURED)
The base emitter is the **revolver PID emitter**: `PIDChainRevolver.next()` emits a brown-hilbert /
`sha16(seed)` PID — a **"200ns-class spawn id"** (`drive-wave-cascade-pipeline-60D-2026-06-03.cjs:30`).
One thread emitting a PID every ~200ns ≈ **5,000,000 PID/s**.

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
| **spindles** | 24 → 100 → 1,000 → 10,000 | each runs its own emit→loop→PRISM |
| **emitters** | 1 → N parallel revolver emitters (divide threads) | N independent streams |
| **per-emit wave** | 1 PID → 1 spindle (1 main + 5 subagents), 1 BEHCS-1024 glyph @ 520:1 | one ~200ns emit materializes a whole subsystem |

→ operator-canon throughput **≈ 1.16 trillion agents / second**. (The ~200ns single-thread unit is
MEASURED in code; the 1.16T multi-emitter rate is OPERATOR-CANON — emitters × spindles × wave/addressing
amortization.)

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
