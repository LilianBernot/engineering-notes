**Date:** 2026-05-22
**Tags:** [Mentorship, Career Development, Being a SWE, Internship, Software Engineering]
**Status:** Published

# Introduction

I started full-time in Paris in November 2025. Three months later, in early March, my manager told me I'd be mentoring an intern joining the team. I had finished my own internship at Datadog six months earlier. The chair I'd been sitting in — the one with the laptop that wouldn't quite configure, the standups I didn't understand, the standing question of "what am I going to say at defense?" — that chair was about to be filled by someone else, and I was the one supposed to help them sit comfortably in it.

This post is two things. First, the journey: what being an intern was actually like, and what changed when I became the mentor. Second, the part that matters more if you're about to mentor someone for the first time: the handful of things I liked as an intern, and the ones I tried to do once it was my turn.

# My Internship Experience At Datadog

## Two Kinds of Internships

There are roughly two shapes an internship can take.

The first is the **solo niche project**: you arrive, you're handed something specific, you have a mentor, and your job is to grind on that one thing for three or six months. Some schools love this shape — it sounds technically deep, easy to grade, easy to defend.

The second is **being part of the team**: you don't get a single big project. You take small tasks, you go to all the meetings, you talk to everyone, you learn how the company actually works.

I had teachers who were supportive of the second kind. I'd already done two internships before Datadog where I took part fully in what the company was doing, and that was the best preparation I could have asked for. I arrived at my third internship knowing what the industry looked like, with my own requirements for what made a company fit me.

The first shape sounds rigorous. In practice, if your niche project gets dropped because you were too slow, or because the company's direction shifted, you're left with a thin résumé and a defense full of caveats. The second shape gives you something more durable: a sense of how the work moves, who owns what, and what kind of company you want to be in next.

If you mentor an intern under a school constraint that pushes the first shape, you can still pull them toward the second. The deliverable on paper is one thing; the experience you give them is another.

## First Days at Datadog

My first week at Datadog, I followed my manager around all day. Every meeting except hiring interviews. I had lunch with him and two other managers he was close with on day one. By the end of the week, I'd been introduced to most of the people who'd matter during my internship — the engineers I'd rely on, the product owners I'd talk to, the cross-team contacts I'd need.

I went to standup (we did it sitting around a table, which I never quite got over). I went to sprint planning, the monthly retro, every recurring meeting. I couldn't say anything for the first two weeks. The script went:

- *Week one:* "Today I attended some internship presentations, set up my People Team account."
- *Week two:* "I ran the service locally, opened my first one-line PR."
- *Week three:* "I found this bug on prod and shipped a fix, deploy is running."

And without noticing it, I went from being a green plant staring at everyone to being someone with something to say.

My first project was so under-specified I didn't know where to start. "See that feature? The data is stored in some old database in a repo the company wants to deprecate." I had to chase the frontend code to find which backend it called, find that backend in another repo that nobody owned, find the database we didn't have access to. I made a lot of brain knots trying to figure out who owned what. One of the backends was so badly intricated that there was no way to test locally — I had to deploy straight to staging to run my database modifications, and I was happy not to fully destroy anything.

I learned more from the *shape* of that project than from finishing it. How the company was wired. Where the load-bearing people were. What "ownership" actually meant.

## Becoming the Mentor

Six months later I was full-time. Three months after that, I was mentoring.

The thing that surprised me most was how recent the intern experience still felt — and how much that helped. I remembered the specific frustration of typing the same command for the fourth time because I hadn't yet aliased it. I remembered not knowing what to say in 1-1s with senior engineers.

What follows is the advice I'd give anyone about to mentor someone for the first time. It's not exhaustive. It's the handful of things that I think actually move the needle.

# Some Opinionated Advices

## The First Week Is Setup Week

The first week is for setup. All of it.

Laptop config. Tooling. Internal sites — the people directory, the codebase navigator, the dashboards, the docs wiki, the support channels. Going through the motions of finding things, even when nothing is broken. The better your intern does this in week one, the faster they'll be in week two, and the less embarrassing it will be in week three when they're still tweaking their shell aliases instead of shipping.

The other half of the first week: meeting the team. Get them in front of as many people as you can. Take notes during those introductions for future use. People at Datadog are — and I think this is true at most good companies — genuinely eager to explain what they do. Most engineers are nice to talk with. Use that.

If your intern leaves week one with a configured laptop and a list of names attached to faces, you've already done a lot.

## One PR a Day

The most controversial piece of advice I give, and the one I keep coming back to: **try to ship one PR a day.**

I expected this to be obsolete by now. AI tools have changed how fast code gets written, how much context a single engineer can hold, how much can land in a single change. I assumed the one-PR-a-day rule would dissolve. It hasn't. The reasons are the same as they always were:

- **Show that you ship.** Asking for PR reviews puts your name in channels. Your coworkers see what you're working on. They learn what they can ask you about. It also keeps you from drifting into a four-day investigation with nothing visible at the end.
- **Get daily feedback.** Not just on code (which I hope you're not writing manually anymore). Feedback on PR descriptions, on naming, on how you communicate. The people you work with most often are the best loop for this, and a PR a day keeps that loop tight.
- **Practice moving forward in small steps.** It's easy to slip into writing a 1000-line PR, especially when AI makes it cheap. Shipping in chunks is better in almost every dimension I can name: easier to find reviewers, easier to incorporate feedback, easier to roll back, easier for production bugs to surface gradually instead of all at once.

If your intern hits one PR a day for the first month, almost everything else takes care of itself.

## Pair Programming Goes Both Ways

I had hour-long pair sessions every day for the first two weeks with my intern. We swapped configurations. I showed her shortcuts. She asked questions that made me realize how arbitrary some of my habits were.

This is one of those activities that sounds generous from the mentor's side but is genuinely two-sided.

I remember an "innovation week" during my own internship where I worked closely with a senior engineer who was mostly working on frontend. We built something together for a week. I walked out of that week with new tools, new ideas, and a noticeably smoother day-to-day workflow. That was someone choosing to spend a week pair-programming with an intern — and it visibly cost them nothing.

Two things I didn't expect about being on the mentor side of pair programming:

- **It makes you self-conscious about your own setup, in a good way.** When you're describing what you're doing as you do it, you notice the friction. You can't find the alias. Your shell history is a mess. The repo organization you've internalized doesn't survive narration. You end up cleaning things up.
- **The intern just got out of school. They've been taught by many people, recently.** They've read things you haven't. They've used languages or libraries you don't know. They'll mention bloggers, classes, tools. Some of that is genuinely useful to you. Don't dismiss it.

The framing I'd push back against is: "I'm helping the intern catch up." A better framing is: we're learning two different things from the same session.

## Delegate Real Work

A recent example: my manager was on-call and wanted to delegate part of it — at least the support questions — to the intern.

At first glance, this looks like classic top-down dumping. The senior person passes the annoying part to the most junior person. But look at what the intern actually gets:

- Exposure to more products in the team than they'd otherwise see.
- A reason to talk to engineers and product owners they wouldn't normally reach.
- Real practice at writing clearly under time pressure (support questions don't wait).
- A working sense of what being on-call actually feels like.

That last one matters more than it sounds. It's a great thing to talk about in an internship defense, and it's a real differentiator if you're trying to convert into a full-time offer.

More generally: **give your intern small tasks that you could do yourself, probably much faster.** It costs you some efficiency in the short term. In exchange, you see how they think, where they get stuck, what they get wrong. You get to be opinionated about how something should be done — which gives you both something concrete to talk about, and often makes you rethink your own habits when you hear yourself defend them. Before the internship helps, your mentee will be an autonomous colleague who you can work with side-by-side.

## Explain the Why

The thing that's hardest to give an intern is context.

It's easy to tell them "go to this meeting" or "read this article" or "talk to this person." It's harder, and more valuable, to say *why*. Why does it matter that they go? What are they meant to take from it? Who should they look out for in the room?

This matters most for the meta-skill of **knowing what to talk about with people more senior than you**.

It's actually a hard problem. You know your life and theirs are different. You know your level of understanding is different. When you talk, they'll follow most of what you say. But when they talk, you'll often not even understand the *point* of their job — because you're trying to grasp a high-level description of something with cross-product, cross-team dependencies, or a technically obscure project that sits far from you in the stack.

This is one of the various things I stated here that I'm actively working on. I don't know how to teach it directly. But I know the antidote to the silence is *context*: "here's why I'm sending you to this person, here's what they care about, here's what would be interesting to ask them about." The mentor's job, more than handing out tasks, is handing out reasons.

## Let Them Impress You

A short note to end on.

You're a few years ahead of your intern, not decades (well, my mentor was but you're probably not). The gap is real, but it's nowhere near as big as it might feel in the early days. Let yourself be impressed by them. They're freshly out of school, intelligent, taught by good people, and they're going to do something you don't expect.

Notice when it happens. Tell them.

# Conclusion

The mentor I tried to be was, more than anything, the mentor I wished I'd had: someone who'd walk me around the office, sit me at the right tables, explain *why* the boring meeting mattered, give me real work, and quietly mention that one PR a day was worth aiming for.

Mentorship isn't a one-way transfer. The version of it that actually works runs in both directions — and the version that works best is the one where the mentor is honest enough to admit the intern is teaching them, too.

If you're about to mentor your first intern: configure their first week, push them to ship daily, pair with them, delegate real work, explain the why. Then watch them, and let them impress you.

*Thanks to Fred, Kévin, Benoit and Tiina, who showed me what good mentorship looked like before I had to do it myself. And to Doaâ — most of what's in the second half of this post is something I figured out while working with you.*

---

*Last updated: 2026-05-22*
