# Genesis Retopo — Showcase

An automated retopology add-on for Blender, built around an autonomous research-and-validation pipeline.

**Status: active development. Not released. Source private.**

This repository is a showcase — output examples, architecture, and the engineering approach. The add-on itself is a commercial product in progress, so its source is not published here.

---

## The honest version, up front

Genesis Retopo does not currently produce topology a professional artist would keep. It produces geometrically faithful output on a minority of test components, and the topology quality bar is not met on any of them.

I know that precisely rather than vaguely, because the project maintains a written audit against its own finished-product specification — eleven completion criteria, each marked MET / PARTIAL / NOT MET with the evidence that decided it. The current audit records no criterion as fully met.

That audit is the reason this is worth showing. The interesting part of this project isn't a finished retopology tool. It's the machinery built to find out, rigorously and without self-deception, whether the tool works yet.

---

## What problem it solves

Retopology — rebuilding a dense or messy 3D mesh as clean, workable geometry — is slow, manual, and one of the least enjoyable parts of a 3D pipeline. Existing automatic tools tend to produce output that's technically valid and practically unusable, and they tend to fail silently: you get a mesh, it looks plausible, and the problems surface later.

Genesis Retopo targets the second half of that problem as hard as the first. Output that can't be trusted is worse than no output.

---

## Showcase

Before-and-after wireframes across eight test assets — treasure chest, overhead crane, vintage cabinet, industrial coffee table, old military compressor, spinning wheel, modular wooden pier, and a sungka board.

> **TODO:** images land in `images/` — see *Adding the images* at the bottom.

Each asset has source and result renders in wireframe, flat, smooth, and shaded, across hero, front, and detail views. The wireframe pairs are the ones that matter — they show the topology change, which is what the tool actually does. Some results are rough. They're included anyway.

---

## How it works

- **Autonomous execution** — the pipeline runs unattended through Blender over MCP, executing a research or build stage and capturing evidence of what it did.
- **Durable state** — work persists across restarts; runs can be resumed rather than redone.
- **One-writer safety** — concurrent writes to shared state are prevented rather than hoped against.
- **Artifact hashing** — outputs are content-addressed so a claimed result can be verified as the one actually produced.
- **Audit trail** — every stage writes a timestamped evidence folder with a gate report. The project currently carries roughly 80 of them.
- **Fail-closed validation** — when a component cannot be retopologized to standard, the pipeline returns *nothing* rather than returning damaged geometry. This is the single most important design decision in the project.

## The validation approach

This is the part I'd most want a reviewer to look at.

- **Independent review rulings** — a separate model inspects results and issues APPROVE / REVISE / REBUILD verdicts. It judges the output; it doesn't author it.
- **Ground-truth certificates that never consult the metric under test** — validation can't be satisfied by gaming the thing being validated.
- **Frozen intrinsic relations** — three topology-defect relations were validated against controls with zero false rejections before being trusted.
- **Sealed blind evaluation set** — a held-out set remains sealed so it can't be tuned against.
- **Negative results recorded as findings** — approaches that were tried and closed off are written down with the reasoning, so they don't get re-attempted. Several architectural directions are explicitly marked closed.

Fail-closed is load-bearing here. In one evaluation round, seven components emitted nothing rather than emitting damaged meshes — and the meshes they would have emitted did contain self-intersections. The user never saw them. That's the mechanism working as designed.

---

## What I designed vs. what agents implemented

Worth stating plainly: this was built with AI coding agents. I defined the architecture, the validation strategy, the gate criteria, and the constraints on what the pipeline was allowed to do unsupervised. A substantial amount of the implementation was AI-generated under that direction, then inspected, tested, and iterated on.

The design decisions that matter — fail-closed over best-effort, independent review, sealed evaluation sets, evidence-before-claims — are mine.

---

## Known limitations

- **Topology quality is the open problem.** Output is not yet at a bar where an artist would keep it without rework.
- **Aggressive reduction is unqualified.** Nothing above ~2.5× reduction has passed evaluation.
- **Test coverage is narrow.** Ten components, most of them procedural fixtures — and those fixtures have been shown not to be representative of real production assets.
- **Density control is unreliable on some inputs**, with delivery underruns against the requested face target.
- **Slender and filament-like geometry is not handled well.**
- **Packaging and release readiness: not started.**

## What's next

The current direction is a constructive mechanism that makes specific named feature classes representable — rims, lips, curls — rather than continuing to search for a metric that predicts which components will fail. That search was tried across five separate candidate quantities, all of which ordered error *within* a component while predicting nothing *between* components. It's been closed off as a direction.

---

## Tech

Python · Blender · MCP · autonomous agent orchestration · Instant Meshes (field solve)

## Licensing

Genesis Retopo is licensed GPL-3.0 — standard for Blender add-ons, since Blender itself is GPL. GPL requires source to be provided to people it's distributed to, not to the world, so the repository stays private until release.

---

## Adding the images

> **TODO (Evan):** from Drive, open `GenesisRetopo_Evidence/SHOWCASE`, use **Download** on the folder — Drive zips it — then unzip into `images/` here. One click beats me pulling ~128 files one at a time through the API.
