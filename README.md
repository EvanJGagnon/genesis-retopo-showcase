# Genesis Retopo — Showcase

An automated retopology add-on for Blender, built around an autonomous research-and-validation pipeline.

**Commercial product in active development. Source private.**

This repository is the showcase — measured results, architecture, and the engineering approach. The add-on's source is not published here.

**[Full interactive showcase →](https://evanjgagnon.github.io/genesis-retopo-showcase/)** — six assets, three shading modes, every pair from the same camera.

---

## Where the project actually is

**Shape preservation is solved. Topology cleanup is what I am working on now.**

Genesis reliably returns geometry that holds its silhouette, keeps its features, and passes hard validity gates — no self-intersections, genus and part count preserved, consistent normals, no degenerate faces. It does that while never touching the original: across all six showcase assets, **source meshes altered: 0**, verified by hash rather than by eye.

What it does not yet do is produce topology clean enough that an artist would keep it without rework. That is the current problem, and it is the one I am actively solving.

---

## How it works — and why the ratios look modest

Genesis works **part by part**. It rebuilds the parts it can prove it improved and preserves the rest byte for byte, so a result is usually a mix: new topology where it earned it, original geometry everywhere else. The "rebuilt" figures below count only triangles Genesis generated — carried-through geometry gets no credit.

That is why the reduction ratios look modest next to the rebuilt counts. The compressor drops only 1.27x overall because **37 of its 61 selected parts failed a quality gate and were kept exactly as they were.** Genesis would rather hand back your original part than a worse one.

---

## Results

All six assets through the shipping build at a 16,000-triangle request.

| Asset | Source → Result | Rebuilt | Parts rebuilt | Reduction | Time |
|---|---|---|---|---|---|
| Treasure Chest | 52,042 → 24,752 | 18,152 (73%) | 22 / 29 | **2.1x** | 9.74s |
| Vintage Cabinet | 29,700 → 14,996 | 9,778 (65%) | 29 / 31 | 1.98x | 6.06s |
| Industrial Coffee Table | 41,300 → 24,340 | 11,296 (46%) | 8 / 12 | 1.7x | 5.89s |
| Overhead Crane | 49,624 → 34,864 | 9,818 (28%) | 23 / 37 | 1.42x | 8.33s |
| Spinning Wheel | 52,023 → 36,959 | 9,994 (27%) | 32 / 65 | 1.41x | 11.4s |
| Air Compressor | 79,042 → 62,234 | 11,178 (18%) | 24 / 61 | 1.27x | 14.71s |

**303,731 source triangles · 70,216 rebuilt · 138 of 235 parts · 0 source meshes altered**

### Before / after

Wireframes in [`images/`](images/), original on the left, Genesis on the right, same camera in every pair.

| Asset | What to look at |
|---|---|
| [Treasure Chest](images/treasure_chest/) | The lid panels: flat spans lose most of their edges while the ornate hinges keep theirs. |
| [Vintage Cabinet](images/vintage_cabinet_01/) | Mouldings and glazing bars survive. The large flat back and side panels are where the triangles came from. |
| [Coffee Table](images/industrial_coffee_table/) | Every slot in the perforated frame is a hole the result has to keep — and it does. |
| [Overhead Crane](images/overhead_crane/) | The girder simplifies down its length; the fittings at both ends hold their density. Density goes where the detail is. |
| [Spinning Wheel](images/spinning_wheel_01/) | Thin spokes and turned legs. 32 of 65 selected parts cleared every gate; the rest were preserved rather than risked. |
| [Air Compressor](images/old_military_compressor/) | The contested one. Flat shading shows real faceting on the tank, but most of that apparent damage is shading, not shape. |

### The limitation, up close

The treasure chest `*_detail_wire.png` pair is the closest look at what the pipeline emits today. The result is aggressively triangulated — it is the clearest proof Genesis is genuinely rebuilding geometry, and the clearest illustration of why the topology-quality bar is not met yet.

Both things are true at once. That is the honest state of the project.

---

## What every result passed

No self-intersections. Genus and part count preserved. Consistent normals, no degenerate faces. Silhouette and surface fidelity inside frozen tolerances. Source objects returned untouched in all six cases, hash-verified.

## The engineering approach

The part I would most want a reviewer to look at:

- **Fail-closed validation** — when a part cannot be rebuilt to standard, the pipeline returns the original rather than damaged geometry. In one evaluation round seven components emitted nothing rather than emitting meshes that did contain self-intersections. The user never saw them.
- **Independent review rulings** — a separate model inspects results and issues APPROVE / REVISE / REBUILD. It judges output; it does not author it.
- **Ground-truth certificates that never consult the metric under test** — validation cannot be satisfied by gaming the thing being validated.
- **Frozen intrinsic relations** — three topology-defect relations validated against controls with zero false rejections before being trusted.
- **Sealed blind evaluation set** — held out so it cannot be tuned against.
- **Negative results recorded as findings** — approaches tried and closed off are written down with reasoning so they do not get re-attempted. Several architectural directions are explicitly marked closed.
- **Autonomous execution with a full audit trail** — runs unattended through Blender over MCP with durable state, one-writer safety, artifact hashing, and a timestamped evidence folder per stage. The project currently carries roughly 80 of them.

## The honest limitation

These are 1.3-2.5x reductions. A per-part safety clamp currently stops Genesis rebuilding any part below 40% of its original density, so the aggressive case — a 60k prop down to 5k — is outside what the current architecture will attempt.

That ceiling is the next thing on the roadmap, not a property of the approach.

## What I designed vs. what agents implemented

Built with AI coding agents — Claude and Codex, each with its own tailored instruction set. I defined the architecture, the validation strategy, the gate criteria, and the constraints on what the pipeline could do unsupervised. A substantial amount of the implementation was AI-generated under that direction, then inspected, tested, and iterated on.

The decisions that matter — fail-closed over best-effort, independent review, sealed evaluation sets, evidence before claims — are mine.

## Tech

Python · Blender · MCP · autonomous agent orchestration · Instant Meshes (field solve)

## Licensing

Genesis Retopo is GPL-3.0 — standard for Blender add-ons, since Blender itself is GPL. GPL requires source to be provided to those it is distributed to, not to the world, so the repository stays private until release.

---

© 2026 Evan Gagnon. All rights reserved.

Published for portfolio review. Not licensed for reuse, redistribution, or derivative works.
