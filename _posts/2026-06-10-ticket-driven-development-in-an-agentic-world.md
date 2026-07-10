---
layout: post
title: "Ticket-Driven Development in an Agentic World"
description: "What a ticket becomes when the agent compiles it — and whether the methodology holds up on cost, complexity, and adoption, measured on an enterprise codebase and the Quorum research vehicle."
date: 2026-06-10
tags: [methodology, agents]
permalink: /quorum/ticket-driven-development-in-an-agentic-world/
---

{% raw %}
Over the last few years, AI has moved from a curiosity — "have you heard about this?" — to a force that is reshaping entire industries. Software development sits at the center of that shift. It was among the first fields to feel the change, and the change runs deeper here than almost anywhere else.

This article opens a series on one narrow but consequential question: how AI changes the *methodology* of building software — not the tooling or the autocomplete, but the process itself. To what extent does agent-driven development actually move the practice? Where does the real benefit come from? And how do you build a process around it that is sustainable, reviewable, and cost-efficient — rather than a novelty that collapses under its own enthusiasm?

What follows are my opinionated answers, and they did not start in a lab. The approach I call ticket-driven development was first put under real probation on a large commercial codebase: a mature enterprise product spanning more than fifty repositories, in languages from JavaScript to Kotlin/Java and C# — some thirty years old, others only weeks old — maintained by a geographically distributed team of roughly ten developers. It was adopted there gradually, tested under genuine production pressure, and the results were good enough to be worth understanding properly. That is what led me, about six months ago, to build [Quorum](https://github.com/ia64mail/quorum): a research vehicle for multi-agent, semi-autonomous software development, where the same methodology could be studied and measured in isolation.

## What is a ticket?

A ticket is a ticket — or it always has been. It is one of the oldest and most familiar artifacts in software development: a technical specification of the work to come, a definition of a unit of work, the requirement a developer sets out to satisfy. Tickets have been with us since the beginning of the discipline and barely qualify as a new idea. What agentic development changes is not the artifact but its role.

> We no longer write the code. We write the ticket — and the agent compiles it.

That single shift is the whole of this article. In conventional practice the ticket is "paperwork around the code": a description of work that someone then performs by typing. In ticket-driven development the ticket IS the work — "source code" written in natural language — and the code becomes its compiled output.

<p align="center">
    <img src="https://github.com/user-attachments/assets/430b88b5-bcfc-42b0-98d2-b806cfbc67b0" alt="Conventional development compiles hand-written source code to an executable; ticket-driven development inserts a human-authored ticket and an AI-agent compile stage ahead of the same source code." width="700">
  </p>

*The yellow highlight marks each pipeline's **product value**. Conventionally it rests on the source code the developer types by hand. In ticket-driven development it moves left — onto the ticket and the AI agent that compiles it — while the developer steps out of the line to drive that agent through engineering.*

The shift is not just where the human types. Conventionally the product's value is locked in the **source code** — the consumed output of human effort. In ticket-driven development that value moves to the **ticket** — the retained, reusable output of the same effort — while the source code becomes regenerable output the agent rebuilds on demand. The human's contribution stops being a spent input and becomes a durable asset.

In an agent-driven workflow the ticket becomes the cornerstone of the process, carrying several distinct loads at once:

- **It is the requirement.** A well-defined statement of the work — the specific slice of business logic or system behavior being addressed — written before any code exists. This is *what* the developer and the product intend to build.
- **It drives the implementation.** The ticket records the decisions already made — approach, structure, constraints, the non-obvious choices and their rationale — so the agent implements rather than improvises. This is *how* the agent compiles it.
- **It is the cheapest point of disagreement.** The ticket is reviewed before any code exists, so design flaws surface as comments on a spec rather than as bugs in an implementation — and changing course costs nothing but an edit. Disagreeing with a plan is cheap; disagreeing with code is expensive.
- **It is the verification blueprint.** Acceptance criteria are concrete, checkable conditions that define "done." Code review runs against the same ticket that drove the implementation — a check of compilation correctness, not an abstract opinion about code quality.
- **It feeds the documentation.** Completed tickets accumulate the knowledge of an entire feature or subsystem, and at milestone boundaries they are synthesized into system documentation — an explicit, agent-assisted, human-reviewed pipeline. This is how project-level documentation stays current semi-automatically.
- **It compounds.** Tickets form a sequential history of every development step, and each new change begins as a new ticket written against that history. As the library grows, drafting an accurate, fully-contextualized ticket takes less and less developer input — the context is already on file.

Beyond the development loop itself, the same library pays off at other levels too:

- It consolidates what is otherwise scattered across meeting notes, chat threads, and commit messages into a single, narrated history of the team's daily work.
- It educates: a new team member — or a new agent — reads the sequence and absorbs both the business requirements and the way the team builds.
- It makes collaboration genuinely asynchronous: a precise ticket replaces the meeting, and a geographically distributed team reviews the artifact on its own schedule instead of hunting for a shared hour.
- It reduces the bus factor: when a developer moves on, the "why" behind their work stays in the repository rather than leaving with them.
- It is reachable by the same search that finds the code: a ticket is a markdown file beside the code it produced, so any keyword or symbol search an agent runs over the repository sweeps the tickets in the same pass — and because the agent compiled the code from the ticket, the two share a vocabulary, so a search for a method or variable name tends to land on the ticket that defined it. 
- It is tool-agnostic: the product knowledge — what was built and why — lives in the repository, not in a tracker. Jira, GitHub Projects, or whatever comes next handles only the disposable process layer — iteration, status, assignment — that the knowledge never depends on. 

Just as important is what a ticket is NOT:

- **Not a copy of the code.** Tickets carry small, focused snippets as hints — a type definition, an interface, a config fragment — never the implementation; the codebase remains the source of truth for "how" ([tickets/README.md](https://github.com/ia64mail/quorum/blob/main/tickets/README.md)). This keeps a focused ticket in the low hundreds of lines — the Quorum library's median is just under 200 — and limits the confusion and code drift that accumulate as a ticket ages into a historical snapshot.
- **Not a deliberation log.** Drafting may explore alternatives, and the ticket may name the ones rejected and why — but it records the final decision, unambiguous and closed. A ticket the agent is expected to compile cannot be open-ended.
- **Not documentation.** Documentation is a living reference, updated as the system evolves; a ticket is a time snapshot, written once and almost never modified. It describes the system's intended transition from state A to state B — and never pretends to describe the current state.

## Does it hold up?

A methodology pitch owes the reader evidence, not conviction. So before closing, four measurements — drawn from Quorum's instrumented history and from the enterprise adoption.

### What it costs

No single ledger answers this question. Three measurement windows are available — the enterprise adoption before and after its agentic transition, and Quorum's instrumented history — and each has a different blind spot. The pre-agentic enterprise era is fully measured: compensation known, output known. The agentic enterprise era has known output and unchanged developer cost, but the AI spend riding on top of those salaries was never isolated. Quorum is the mirror image: every API call is metered to the cent, but the author's time is an after-hours budget no payroll records. A direct comparison between any two of the three would be inaccurate — held side by side, they still bracket the scale of the shift reliably.

Velocity first, because it needs no cost model at all. The same measurement applies to all three windows: lines of merged change per developer-month, counted identically (additions plus deletions on default branches), with machine-generated bulk commits excluded on all sides.

| Period | Method | Merged lines / dev-month |
| --- | --- | --- |
| Enterprise, pre-agentic | conventional | 568 |
| Enterprise, agentic | ticket-driven, one agent per developer | 5,955 (~10×) |
| Quorum, research project (May 2026) | ticket-driven, orchestrated agent fleet | ~30,300\* (~53×) |

\* *Ticket prose is counted on both sides — the enterprise team co-locates ticket files in pull requests just as Quorum does. Measured from a single after-hours author; the instrumented seven-ticket window projects to ~29,000, so the month's figure is a steady cadence, not a sprint.*

The middle row is the important one. Those are the same developers as in the row above — same product, same salaries; the only thing that changed is the method. They moved to ticket-driven work: at first on tickets I wrote for them, later on tickets they wrote themselves. Output grew tenfold. The third row changes the setup rather than the people — one author driving a whole fleet of role-based agents instead of pairing with a single assistant — and that buys roughly another factor of five.

Now cost — and only two of the three rows can be priced honestly. The enterprise salaries are known for both eras, but what the organization now spends on AI on top of them was never tracked, so the middle row stays open-ended. Quorum is the opposite: the AI bill is metered to the cent, and the human side is an assumption — a few hours of my attention, priced at double a senior salary to keep the estimate honest.

| Period | Developer cost | AI cost | Cost per merged line |
| --- | --- | --- | --- |
| Enterprise, pre-agentic (conventional) | \$5–10K/month, assumed | — | \$9–18 (approximated) |
| Enterprise, agentic (ticket-driven) | unchanged | not isolated | ≥ \$0.8–1.7 (salary-only floor) |
| Quorum, research project (ticket-driven fleet) | \$10-15K/month, assumed | \$45.73, metered | ~\$0.21 (approximated) |

The baseline itself is no anomaly: decades of industry measurement, from Capers Jones to the Cutter Consortium and COCOMO, put a normal senior team at a few hundred lines a month and roughly \$10–20 per line — exactly where the pre-agentic row sits.

Read the outer rows together: a line of code that cost \$9–18 the conventional way comes out at roughly twenty cents — and even then, the AI bill is the small part of the figure; most of it is still the human. **That is the whole cost story of ticket-driven development: you keep paying the developer, the developer produces an order of magnitude more, and the AI riding on top costs pennies**. In these measurements, the magnitude is tenfold for a team that changed nothing but its method — and roughly fifty-fold for an agent fleet in the hands of one practiced author.

### Whether quality erodes

The most common objection to agent-written code is entropy — surely code produced at ten times the speed rots just as fast. Two metrics answer it together, because each is blind without the other. Halstead Volume treats code as vocabulary: it counts the distinct operators and operands and how often they recur, condensing a codebase into one number — how much information it carries. Divided by lines, it becomes a density: how much each line "says". Cyclomatic complexity measures what Volume cannot see: the number of independent decision paths through each function — how branchy the logic is, and roughly how many tests it takes to cover. Density alone is ambiguous: a line can be packed because it is expressive, or packed because it is overloaded. Branching resolves the ambiguity — dense and branchy is spaghetti; dense with simple control flow is concise. Both measurements were run over the same three windows as before, with identical tooling end to end.

Density first, measured across each codebase's full history:

| Period | Volume per line | Volume per file |
| --- | --- | --- |
| Enterprise, pre-agentic (conventional) | 47 → 63, then flat | ~4,400 |
| Enterprise, agentic (ticket-driven) | ~46 → 71–74, still rising | ~7,200–8,700 |
| Quorum, research project (ticket-driven fleet) | 43 → 79, still rising | ~9,400 |

Hand-written code ramps up early and then holds its density for years — it grows larger, not denser. Ticket-driven, agent-compiled code is markedly denser per line and per file, and keeps getting denser. On its own that fact could be read either way, which is exactly the ambiguity the second metric exists to resolve.

So, branching — a snapshot of each codebase as it stands today:

| Metric | Enterprise, pre-agentic | Enterprise, agentic | Quorum, research project |
| --- | --- | --- | --- |
| Average complexity per function | 1.28 | 1.26–1.30 | 1.19 |
| Median / 99th-percentile complexity | 1 / 7 | 1 / 6–7 | 1 / 5 |
| Share of functions above complexity 10 | 0.5% | 0.2–0.6% | 0.3% |
| Functions per file | 4.0 | 6.6–10.0 | 10.3 |
| Average function length (lines) | 11.4 | 10.9–11.8 | 8.8 |
| Average parameters per function | 0.51 | 0.18–0.26 | 0.09 |

The extra density carries no branching penalty: complexity per function is statistically identical across all three windows — low everywhere, with the overwhelming majority of functions a straight line of logic. And reading down the columns, the pattern is monotonic across the methodology spectrum — conventional, ticket-driven, ticket-driven fleet: **the deeper into ticket-driven development, the simpler the control flow, the smaller the functions, the leaner the signatures, the more decomposed the files.** The density is expressiveness, not tangle. One scope note, which is really the point of this article: every agentic window in these tables is ticket-driven. The measurements say nothing about agents improvising from free-form prompts — they describe what agents produce when they compile reviewed tickets.

*Caveat:* the two instruments answer different questions on different clocks — the density figures span each project's whole history, while the branching snapshot describes only the code as it stands today. A manual baseline set against a handful of independent ticket-driven codebases is a consistent trend, not a proof. And neither metric sees readability, test coverage, or architectural soundness — this is strong structural evidence against erosion, not a full quality verdict.

### How far a ticket library takes an agent

The two measurements above describe the development loop: what it costs to compile tickets into code, and what that code looks like afterward. This one is about everything else. Tickets compound — each one adds context for the next — and past some depth the library stops being a development aid and becomes the operating knowledge of the whole project: deep enough to drive work at levels the tickets were never written for.

One episode from the enterprise adoption measures the effect directly. The task was not implementation but verification: confirm that a freshly built cross-service feature works end-to-end, in a live multi-tenant test environment, across five repositories/apps. The human steering for the entire session amounted to a few sentences of intent — about eighty words in total, none of them procedural: not which logs to read, not the expected event sequences, not how the feature was wired, not even how to trigger it. Everything operational — environment access, the exact event topics and payload schema, the log signatures to grep on each service, a ready-made acceptance matrix — the agent reconstructed from the ticket library: ~500 tickets accumulated across those five repositories over 12 months of the practice. Before running the test, it predicted the system's event fan-out **byte-for-byte** from a single ticket, and the live trace confirmed it.

That asymmetry — a paragraph of intent in, a five-repository verification plan out — is what the cost table cannot show. The library converted the agent's work from discovery into verification: nearly every action was a check against a ticketed expectation rather than open exploration. Against an empty library, the same eighty words would have bought a multi-hour reverse-engineering exercise, with a human feeding in by hand every operational detail the tickets supplied here. **The deeper the library, the higher the level at which a few words of intent can steer the project — not just implementing the next ticket, but verifying the system those tickets built — because everything beneath the intent is already on file.**

### Whether developers accept it

Nine of ten developers — front-end and back-end specialists across Java/C#, JavaScript/TypeScript, and DevOps/Terraform stacks, spread over three geographically distributed teams — settled into the workflow over a roughly three-month adoption period and built stable, repeatable habits: ticket first, review before implementation, delegate to the agent, review alongside the agent. The most telling observation: **none of them let go of the work once the ticket was written.** They stay close to the code and verify everything — they simply no longer type most of it.

*Caveat:* this is the author's first-person observation of his own teams, not instrumented data.

## References

The measurements above rest on long-established methodology. The primary sources, and the tools that apply them:

- Maurice H. Halstead, *Elements of Software Science*. Elsevier North-Holland, 1977 — the origin of the Volume and Difficulty metrics used throughout "Whether quality erodes."
- Thomas J. McCabe, ["A Complexity Measure"](https://doi.org/10.1109/TSE.1976.233837). *IEEE Transactions on Software Engineering*, SE-2(4), 1976 — cyclomatic complexity, including the original recommendation of 10 as the per-function warning threshold.
- Arthur H. Watson and Thomas J. McCabe, [*Structured Testing: A Testing Methodology Using the Cyclomatic Complexity Metric*](https://www.mccabe.com/pdf/mccabe-nist235r.pdf). NIST Special Publication 500-235, 1996 — that threshold's standardization in testing practice.
- Capers Jones, *Applied Software Measurement: Global Analysis of Productivity and Quality*, 3rd ed. McGraw-Hill, 2008 — the few-hundred-LOC-per-month productivity band cited in "What it costs." Jones himself later warned that line-based comparison rewards verbosity and punishes deletion; this article uses the band as a sanity bracket, not a target.
- Barry W. Boehm, *Software Engineering Economics*. Prentice-Hall, 1981 — the COCOMO cost model behind the per-line estimates cited in "What it costs."
- Cutter Consortium benchmark studies — the per-delivered-line cost band for senior teams at standard rates.
- [lizard](https://github.com/terryyin/lizard) — the open-source analyzer behind every cyclomatic-complexity figure in this article.
- [`tools/entropy-report`](https://github.com/ia64mail/quorum/blob/main/tools/entropy-report/README.md) — Quorum's Halstead tokenizer and per-commit aggregation pipeline; its cyclomatic companion drives lizard over the identical source set, so both metrics describe exactly the same code. The same toolchain, unchanged, produced every measurement in every window.

*The datasets behind the enterprise rows are anonymized internal measurements; they are not published.*

{% endraw %}
