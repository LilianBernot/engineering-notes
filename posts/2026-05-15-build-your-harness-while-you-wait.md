**Date:** 2026-05-15
**Tags:** [AI, Developer Productivity, Software Engineering, Context Switching, Claude, Flow]
**Status:** Published

## Introduction

A few weeks ago, I sat down with a colleague for our weekly 1-1. He came in visibly exhausted — not from a late night debugging, not from an incident. He'd been on vacation and had come back to four active Claude sessions waiting for him, each one pulling in a different direction.

"I always told people not to do this".

I'd heard this before. But sitting in that room, I realized we were looking at a new kind of developer fatigue — one that didn't exist two years ago.

## The New Speed Limit

In 2023, developers were tired from staying up until 3am reading through StackOverflow for a two-line fix, or building entire features from scratch to satisfy a PM's request. That kind of exhaustion had a shape to it: deep, slow, earned.

In 2026, the exhaustion looks different. It's not from doing too little — it's from doing too much, too fast. A new feature now takes three good lines of prompting. The bottleneck isn't execution anymore; it's context switching.

I walk into our office and hear more people complaining about feeling scattered than about being stuck. The challenge has shifted from "how do I build this?" to "how do I stay coherent while building five things at once?"

## The Babysitting Trap

There's a mode I see a lot of colleagues still in, and I was there too not long ago: babysitting Claude. Accepting every file edit, rename, commit, and test run. Sitting in front of the screen, approving actions one by one.

This is a waste of the most valuable thing you have: your attention.

For a few weeks, in the repo I work in most, Claude was running complex Bazel queries that took forever. I kept multiple sessions open just to absorb the wait. At some point I actually looked at what was happening: it was running commands at the repository root level, which is catastrophically heavy in a monorepo. I spent an hour explaining Claude how to scope its commands — package level first, then service or domain, never the full repo.

That one hour saved me dozens.

## Build Your Harness While You Wait

This is the reframe that changed how I work: **use latency as leverage**.

When an agent is doing something long — running tests, building a target, generating protobuf files — that's not dead time. That's configuration time. It's the 20% of my AI time I now deliberately protect. The remaining 5% I keep for research: what's new in the AI world, what are my colleagues trying, what's worth stealing for my own workflow?

Think of it like early career progression. As a newcomer, you have low privileges. You move carefully, ask for approval often. As you grow, you earn operational rights, architectural trust, autonomy. AI sessions are the same: untrained sessions are expensive to babysit. Trained sessions — with the right permissions, the right context, the right guardrails — run themselves.

We don't want to micro-manage. We want to configure, then delegate.

## The Notifications Tax

One underrated part of this: notifications.

I turned off WhatsApp emoji reaction notifications months ago. Every Slack channel I'm in is muted except for PMs with my intern, my manager, and the handful of coworkers I might be on-call alongside.

This sounds antisocial. It's a trade: I respond more thoughtfully and less reflexively. The pings that reach me matter.

Boredom, it turns out, is useful. The moments where I step away from the screen — tidy something up, stare out the window — are where I do my best thinking. I anticipate what the agent will do next. I kill the process, refine the prompt, restart with a better question. The idle second is where the edit happens.

## On Git Worktrees (and Why I Dropped Them for now)

I got excited about git worktrees for a while. The idea is elegant: while one agent works on branch A, spin up another one on branch B. No blocking, pure parallelism.

In practice: more context switching, not less.

Worktrees in a large monorepo are slow to spin up. You have to track which clone corresponds to which task, manage rebases across branches, stack PRs in ways that are genuinely hard to reason about. Some colleagues just maintain two clones of the repo — which is roughly the same problem. I tried letting the AI manage the worktrees for me. That was worse.

There are legitimate uses — deploying something while keeping the working tree open, running long `bazel` or `yarn` commands for auto-generation — but for most day-to-day work, the overhead isn't worth it. The overhead is cognitive, not computational.

## The Comeback Sprint

Three weeks of sick leave. A half-finished feature I cared about. A week of on-call the moment I returned.

When I finally got two free days to work on it, I was four weeks out from the last time I'd touched it. I was also worried someone had moved on without me.

They hadn't. So I started.

I had notes. I had saved links, database queries, context scattered across docs and Slack threads. AI agents helped me reassemble all of it — regroup the data, spec the implementation step by step, proceed, test. By end of day one, I had shipped four PRs. One of them wasn't directly on the feature — it was tooling. That one hour of config work saved hours in the days that followed.

Day two: three more PRs. Done.

A year ago, that feature would have taken me two weeks. Minimum. When I sent it for review, I already knew it was clean. Approvals came fast. I moved on.

The feeling of finishing ahead of your own plan, when you had every reason to expect to be late — that's the feeling I'm chasing.

## Flow, Not Orbit

I read a Forbes piece recently that argued "Orbit is the new Flow." The idea: agentic AI rewards context-switchers, people who keep many things in motion at once rather than diving deep into any single one.

I'm not convinced.

Flow — the uncomfortable, focused state where you stop noticing the room around you — is still where I do my best thinking. When I come out of it, complex things are in place, cleanly. I just have to document what I did, transfer context somewhere durable.

Orbit might scale better. But I'm not sure yet that it scales better *for me*. The risk is creating content and context faster than you can actually ingest or ship it.

The goal isn't to have many things moving at once. It's to have the right things moving — and to actually finish them.

## Conclusion

The new developer fatigue is real, but it's manageable. It's not about doing less — it's about doing it with intention. Protect your focus. Use the wait to improve the harness. Mute the noise that doesn't matter. Ship things completely before starting the next one.

We're still engineers. We're just working at a different speed, with different constraints. Learning to configure the machine — including the AI — is the new leverage.
