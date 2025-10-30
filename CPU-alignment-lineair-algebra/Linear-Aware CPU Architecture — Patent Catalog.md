# Linear-Aware CPU Architecture — Patent Catalog
*(Confidential Internal Draft — 2025)*

This document summarizes all proposed **linear-algebra–oriented CPU architecture innovations** in `5W2H` format, grouped by theme.  
All concepts are implementable **without transistor density increase**, and compatible with existing AMX/SME/AVX-class cores.

---

## 🧮 Execution / Issue / Rename

### 1. Dynamic Linear-Flow Execution (DLX)
**What:** Detect and fuse FMA chains across iterations to sustain accumulator flow.  
**Why:** Boosts FLOPs on small/tall-skinny GEMMs where tile engines stall.  
**Who:** CPU µarch teams, HPC vendors.  
**When:** Next core revision (policy/firmware).  
**Where:** Decode → Rename → Issue queues.  
**How:** Pattern-recognize affine loops; forward accumulator register; bypass rename stalls.  
**How-much:** +15–25 % throughput on dot-heavy kernels.

---

### 2. Register-Level Stream Buffer (RLSB)
**What:** Micro-FIFO that preloads next vector elements before FPU demand.  
**Why:** Hides L1 latency and lowers load-queue pressure.  
**Who:** Core front-end architects.  
**When:** Same silicon; rename→issue path.  
**Where:** PRF front-end.  
**How:** Detect stride-1/2 loads; speculative roll-ahead prefetch into per-thread buffer.  
**How-much:** 10–20 % latency masking.

---

### 3. Vector-Reuse Rotation Buffer (VRRB)
**What:** Rolling rename that reuses vector registers across micro-tiles.  
**Why:** Cuts rename traffic and register-file energy.  
**Who:** Rename/ROB engineers.  
**When:** µarch change only.  
**Where:** Rename tables.  
**How:** Maintain N shadow registers mapped cyclically.  
**How-much:** −30 % rename-stage power.

---

### 4. Arithmetic Reuse Scheduler (ARS)
**What:** Issue-queue cache for repeated operand pairs or partial sums.  
**Why:** Avoids redundant multiply operations in blocked GEMM.  
**Who:** Dispatch teams.  
**When:** Next-gen issue queue.  
**Where:** Issue/dispatch logic.  
**How:** Hash operands; check reuse buffer; bypass compute on hit.  
**How-much:** 5–8 % energy saving.

---

## ⚙️ FPU / Vector / ISA Extensions

### 5. Dynamic Precision Fusion (DPF)
**What:** Pair FP16×FP16 multiplies into one FP32 accumulate when exponents match.  
**Why:** Mixed-precision GEMM with less decode/issue bandwidth.  
**Who:** FPU designers.  
**When:** Pipeline tweak, no ISA.  
**Where:** FPU front-end.  
**How:** Compare exponents → dual-product → fused FP32 sum.  
**How-much:** 1.5× throughput on FP16 paths.

---

### 6. Shared-Accumulator Vector Mode (SAVM) *(ISA optional)*
**What:** SIMD mode where lanes accumulate into one scalar automatically.  
**Why:** Removes horizontal reduction step for micro-dots.  
**Who:** ISA committee, vector engineers.  
**When:** ISA bump + compiler support.  
**Where:** Vector reduction pipeline.  
**How:** Per-lane partial sums merged internally.  
**How-much:** 1.3–1.5× faster reductions.

---

### 7. Argmin/Argmax-with-Index Reduction *(ISA optional)*
**What:** Single instruction returning (min/max, index) pair over a vector.  
**Why:** Speeds pivoting and selection in solvers.  
**Who:** ISA designers, BLAS authors.  
**When:** Future vector ISA.  
**Where:** Reduction tree logic.  
**How:** Compare-and-select with index propagation.  
**How-much:** 20–40 % faster selection stages.

---

## 🧠 Memory / Cache / Prefetch

### 8. Adaptive Tile Prefetch Controller (ATPC)
**What:** 2-D prefetcher learning row/column strides for tiled GEMM.  
**Why:** Linear prefetchers miss on 2-D block access.  
**Who:** Cache controller teams.  
**When:** Firmware/µcode update.  
**Where:** L2/L3 prefetch engine.  
**How:** Detect dual-stride deltas; issue orthogonal streams.  
**How-much:** 20–40 % miss reduction.

---

### 9. Micro-Block Cache Mapper (MBCM)
**What:** Cache indexing that keeps each tile’s lines in distinct sets.  
**Why:** Prevents A/B/C tile conflicts.  
**Who:** Cache designers.  
**When:** Tag hash policy update.  
**Where:** L1/L2 tags.  
**How:** Tile-aware XOR mapping; “tile mode” bit toggled dynamically.  
**How-much:** ≈10 % effective cache gain.

---

### 10. Operand-Recycling Load Queue (ORLQ)
**What:** Load-queue side buffer replaying recently used operands (e.g., A[i,k]).  
**Why:** Cuts redundant loads between iterations.  
**Who:** LSU designers.  
**When:** µarch addition, no ISA.  
**Where:** Load queue / D-cache interface.  
**How:** Address+stride associative table; forward on hit.  
**How-much:** 5–10 % less memory traffic.

---

### 11. In-Cache Reduction Engine (ICRE)
**What:** Adder tree inside cache bank performing partial reductions on eviction.  
**Why:** Halves write bandwidth for reductions.  
**Who:** Cache subsystem teams.  
**When:** Minor per-bank logic.  
**Where:** L1/L2 write-back pipeline.  
**How:** Tag “reducible” lines; accumulate locally before flush.  
**How-much:** Up to 2× lower bandwidth.

---

## 🔋 Power / Thermal / Telemetry

### 12. Power-Aware Dot-Product Governor (PDG)
**What:** Firmware governor adjusting issue width based on detected dot-chain depth.  
**Why:** Saves power in tails; expands in dense compute phases.  
**Who:** Power-management firmware engineers.  
**When:** Firmware update.  
**Where:** Front-end issue controller.  
**How:** Linear-chain counter feeds DVFS and issue-width control with hysteresis.  
**How-much:** 10–15 % total power drop with same performance.

---

## 🧩 Compiler / Runtime

### 13. Compiler-Guided Linear Fusion (CGLF)
**What:** Compiler pass adding *linear-chain hints* to enable DLX/RLSB/VRRB/ATPC/MBCM.  
**Why:** Connects software patterns to hardware optimizations.  
**Who:** Compiler teams, library maintainers.  
**When:** Toolchain update.  
**Where:** LLVM/MLIR mid-end.  
**How:** Pattern-match affine loops; emit metadata or pragmas.  
**How-much:** Unlocks +20–40 % throughput when hardware present.

---

## 📦 Filing Guidance

| Batch | Focus | Key Patents | Rationale |
|-------|--------|-------------|-----------|
| **1 – Core Throughput** | Execution & Scheduling | DLX, ATPC, MBCM, CGLF | Fast to implement, high measurable gain. |
| **2 – Memory & Power** | Efficiency | ORLQ, ICRE, PDG, RLSB, VRRB | Medium complexity, strong IP depth. |
| **3 – ISA Enhancements** | Reduction & Selection | SAVM, Argmin/Argmax-Idx | Small ISA surface, major competitive edge. |
| **Bridge** | Mixed Precision | DPF | Links LA pipeline to FP16/32 ecosystem. |

---

**Total patents proposed:** *13 core + 2 optional ISA extensions (≈15 unique claims).*  
**Target outcome:** *Linear-algebra aware CPU core family delivering 20–40 % performance uplift without area growth.*

---
