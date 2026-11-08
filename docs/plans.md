# Plans and ideas

This page lists what is planned or has been floated. **None of it is promised and no dates are given.** Where something is only an idea, the page says so. Where something has a work item and an owner, it says that instead.

## The end result: the moon and the solar system

The longest line in this repository runs from the deep past to the sky around the Earth. It is a direction, not a schedule, and every step on it either exists, is planned with an owner, or is only an idea.

| Step | What it is | Status |
|---|---|---|
| The time globe | The Earth through time: ice, coastlines and the places where people built, worshipped and suffered, each entry graded for evidence | Live, still growing layer by layer |
| The moon in the globe's sky | A visible moon, drawn larger than true scale, and labelled as such | Live |
| The solar-system kit | One reusable kit and one manifest, with one site per body | Mars and Mercury built through it; the rest is planned |
| The fly-through | Orbits and comets that blend into the Earth globe, with a zoom bar, a statistics tab and a scale switch that says what you are seeing | Planned, with an owner |
| One site, three lenses | Deep time, the present and the solar system on one clock | Idea, parked with a reason |
| The moon as a place | Surface, missions and a lunar timeline | Idea only |

The direction this table points to is one continuous view: from what the Earth looked like when the ice retreated, out through the moon and the planets, with the same rule on every layer: every fact carries a source and says how sure it is. Nothing on this table is promised for a date.

## The solar system

The owner asked for the solar system to be built: every planet and major object its own website, interactive, later linked from one site, with the design built from the start to allow that.

- **One kit, many bodies.** The design that came out of that order is one reusable kit with a thin instance per body, not ten hand-built pages. A single manifest file lists which bodies exist, has one named writer, and is read by the sites at runtime, so that changing a body means changing one file in one place and never a copy in the shared canon (a copy would fork silently the first time either half was patched).
- **Upkeep:** the kit's pages get regular accessibility passes (colour contrast measured against the standard) and changes to them go out under a signed-round rule.
- **Status:** the kit exists and has been used to build the first bodies (Mars and Mercury were built through it, which is how a coordinate-convention trap in one data source was found before it could mirror every body built after it). A fly-through of the solar system, with orbits and comets that blend seamlessly into the Earth globe, with a zoom bar, a statistics tab and a scale switch that says what you are seeing, is a planned item with an owner and a long trail, and is not finished.

## The moon

The sky of the time globe already shows a visible moon. The default rendering of the engine draws the moon at true scale and distance, which is a few pixels, so the globe draws its own larger one, and says so. Any deeper use of the moon (the surface, the missions, a lunar timeline) is an idea, not a work item.

## One site, three lenses

A merged site was proposed: **one clock and three lenses**. Deep time (the time globe), the present (a live view of the world with sources and freshness labels on every layer), and the solar system around it. It exists as a parked item with a reason. The pieces are built on the same engine, which is the whole argument for it, but merging them is a large step with its own questions about hosting, cost and exposure, and it is not under way.

## More layers on the time globe

| Layer | Status |
|---|---|
| Faith places and sacred sites, with neutral wording for every tradition | The filter and the first tagged places are live; more are being added in groups |
| Submerged landscapes such as the North Sea basin | The drowned river channels of Doggerland are live as a layer, off by default |
| Ancient royal roads and courier systems | Planned |
| Pilgrimage routes as lines | Planned |
| Ancient-DNA finds as a small and careful layer | Planned |

No dates are promised for any of them.

## For the fleet itself

- **An independent checker.** Bringing back a checker on a different model family, when the budget and the owner's decision allow.
- **A restore mode for the command filter**, so that a bad release can be rolled back by a documented, proof-checked step. Built, rehearsed on a scratch copy and used for real on a seat.
- **Shorter, cheaper nightly checks.** The nightly gate has grown from minutes to well over an hour. The work is to read where the time goes and to split cheap guards from expensive proofs.
- **A second party for each seat's critical tools.** Today a second party cannot act on another seat's files, by permission design. The honest state is "no second party yet", and it will stay that way until a trust-boundary decision is recorded.

## What we deliberately do not plan

- An OSINT or red-team lab exists only as a design. It is not built and is not on a schedule.
- Automation of anything that does not yet work by hand and demonstrably repeat. **Do not automate chaos.**
