# 🧩 Research Validation — Linear-Algebra for CPU Architecture  
*(2025 draft — internal validation summary)*  

---

## Overview
This document validates the **Linear-Aware CPU Architecture** concepts against current public and industry research in linear-algebra acceleration for CPUs.  
Each **impact vector** compares (1) current state of the art vs. (2) our high-level novelty.

---

| **Impact Vector** | **Recent Public Research / Industry Status (Linear-Algebra, CPU)** | **Our High-Level Novelty (What’s Missing Today & How Ours Differs)** |
|--------------------|--------------------------------------------------------------------|-----------------------------------------------------------------------|
| **Throughput (GEMM / dot / small & tall-skinny)** | CPUs now expose matrix/tile engines and wider vectors: **Intel AMX** (INT8/BF16/FP16 and “COMPLEX” TMUL variants) with tutorials on mixed-precision linear algebra; **Arm SME** (matrix tiles over SVE) with recent performance studies vs SVE; **AVX10.1** unifies/converges vector ISA across P/E cores; libraries (e.g. *oneMKL*) autoselect ISA. These mainly target dense tiles and FP16/BF16; small/tall-skinny throughput still needs bespoke kernels.<br/>🔗 intel.com +5 Fixstars Tech Blog +5 docs.nrel.gov +5 | Our linear-aware core features keep silicon constant yet raise sustained FLOPs outside the “big dense tile” regime: **DLX** (fuses FMA chains across iterations), **RLSB/VRRB** (register-level streaming & rolling rename for micro-tiles), **SAVM** (shared-accumulator vector mode) for fast micro-dots, and **DPF** (on-the-fly FP16×FP16→FP32 fusion). None of these appear in AMX/SME/AVX10 docs; they are **dispatch/scheduler/pipeline** innovations that lift small, batched, tall-skinny, and reduction-bound kernels. |
| **Memory efficiency (tiling, prefetch, cache conflicts)** | Active work optimizes prefetch for linear/regular patterns (e.g. multi-strided transforms) and cache-aware GEMM tuning (tiling, prefetch knobs, CCP). Classic research on conflict-avoiding caches highlights set-mapping pitfalls for matrix layouts. But CPU prefetch/caches remain largely **1-D stride/temporal**, not tile-semantic.<br/>🔗 backoffice.biblio.ugent.be +3 arxiv.org +3 SpringerLink +3 | We introduce **tile-semantic memory machinery** without extra transistors: **ATPC** (2-D tile prefetcher that learns row/col walks), **MBCM** (micro-block cache mapper keeping A/B/C tiles distinct), **ORLQ** (operand-recycling load queue reusing A[i,k] across B[k,j]), and **ICRE** (in-cache partial reductions on write-back). Public work doesn’t expose hardware policies specifically keyed to GEMM tile choreography at this granularity. |
| **Power efficiency (DVFS & linear phases)** | DVFS/power governors are mature but utilization/phase-driven (HPC policy tuning). Energy papers for sparse/LA workloads emphasize **code-level** transforms, not CPU governors that understand dot-chain depth or reduction density.<br/>🔗 ResearchGate +1 | We propose **PDG (Power-aware Dot-product Governor)** that widens/narrows issue width based on live linear-chain depth, and **DPF** to keep compute dense while avoiding decode/issue overhead. This is **telemetry-driven scheduling** for LA, not generic DVFS — absent in public CPU policies. |
| **ISA uplift (optional, not required for baseline)** | Roadmaps add tiles & converged vectors (**AMX/SME; AVX10.1**), plus dot-product/VNNI for int/bfloat paths; libraries dispatch per ISA. No standard ISA features exist for **shared-accumulator reductions**, **argmin + index** reductions, or **ISA-visible micro-dot** modes for small/batched LA on CPUs.<br/>🔗 cdrdv2-public.intel.com +2 developer.arm.com +2 | Our optional ISA adds **SAVM** (vector lanes accumulate to one scalar without horizontal steps) and **argmin/argmax-with-index** reductions for block selection/pivoting. These are **LA-native primitives** complementing tile and vector units; no public x86/Arm ISA currently offers them. |

---

### Bottom Line
Current CPU research excels at **large dense tiles** and **mixed precision** (AMX/SME/AVX10), plus general prefetch/DVFS improvements.  
Our proposals specifically target **underserved linear-algebra regimes** — *small, tall-skinny, batched, tightly-reduced kernels* — through **micro-architectural scheduling**, **cache/indexing innovation**, and **minimal ISA assists** that **no current literature or vendor documentation** yet covers.

---

*End of file – internal research validation (Linear-Aware CPU Architecture, 2025).*
