---
title: "Five lessons from building, serving and stopping an agent"
date: 2026-11-24
authors: ["Shashwat Rai"]
summary: "The broader lesson is that effective agents require coordinated decisions about execution, reasoning, memory, feedback, and time. Improving any one component in isolation is insufficient; what matters is identifying the constraint that currently limits successful task completion and designing around it."
tags: ["agent-design", "serving", "caching", "evaluation", "terminal-bench"]
draft: false
---

The assignments have been very engaging and educational, and I have learnt a lot of lessons to even summarise in a blog post. The first lesson, I would say, is that reliability is an engineering problem. My first agent improved from solving 1/10 tasks to 7/10 through better tool-call handling, error recovery, verification, and patch management. The challenge was not always getting the model to generate the right solution but ensuring it could execute and submit that solution reliably.

<!--more-->

The second lesson was that the unit of optimisation in this setting is a solved task which brings in a different calculus from a model request. In the second assignment, I introduced parallel workers, model tiers, and budget-aware scheduling, reaching 18/26 solved tasks. Disabling reasoning reduced request latency but hurt overall performance because the agent needed more attempts. This demonstrated why inference speed alone is a poor measure of agent efficiency.

Next, that memory can so often be the constraint for concurrency and not just computation, while computation drives most conversations. Serving the model through SGLang exposed KV-cache contention: too many active conversations caused repeated context eviction. Memory-aware admission control increased prefix-cache reuse from 48% to 91%. Interestingly, disabling reasoning now helped, reducing median request latency from 18.6 to 2.2 seconds. The optimal configuration depended on the underlying infrastructure.

Feedback telling the agent how it can improve was another interesting lesson. In the final assignment, I removed the intermediate hidden-test evaluation and adapted the agent to both software-engineering and Terminal-Bench tasks. The agent performed well on Terminal-Bench but struggled more with software-engineering tasks, highlighting the importance of reliable verification rather than relying on the model's judgement of completion.

Lastly, how powerful stopping as an optimiser can be. Allowing difficult tasks to consume their entire time budget often produced diminishing returns. By using observed completion times to inform stopping policies, I reduced one Terminal-Bench run from roughly 80 minutes to 31 minutes while retaining 28 of 31 solved tasks.

The broader lesson is that effective agents require coordinated decisions about execution, reasoning, memory, feedback, and time. Improving any one component in isolation is insufficient; what matters is identifying the constraint that currently limits successful task completion and designing around it.

## AI use

I used Claude Code (Claude Opus) throughout Assignments 2-5 to write, test and run the agent and serving code and to analyse the runs. I wrote this post myself; Claude Code only put it into the blog's template and opened the pull request.
