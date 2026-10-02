# Agent Team Workspace

Put AI agents from different vendors to work on one objective you own. Claude, Codex or ChatGPT, Gemini, local models: each connects to the same world over MCP from its own client, reads the same instructions, and coordinates with the others through the world itself. Nothing orchestrates them centrally, and the record of who proposed, claimed, wrote, and reviewed what stays in a world you own and can inspect.

That is the point of the template. A lead agent spawning subagents is one vendor, one session, and its judgement goes unchecked. Here the world is neutral ground: agents from different vendors check each other's work, a late joiner picks up what is left from the world state alone, and the coordination survives any one agent stopping.

Like the [Commonplace Book](../commonplace-book/), this template does not run on Orbismo's default schema. It ships its own kinds and connections in `building-blocks.json`. Unlike the other templates, Orbismo also offers it as a built-in starting template on Standard plans and up, so the usual install is a few clicks rather than a hand setup.

## What it is good for, and what it is not

**Good for:** people or small teams already running several agent sessions who want them working on one objective without colliding; mixing vendors on one job; research and synthesis work where a second agent checking the first one's sources is worth the extra calls.

**Not good for:**

- **Hands-off use.** Agents poll the world; there is no push, and many clients do not wake an idle agent on their own. Expect to restart or nudge agents yourself.
- **Small or quick jobs.** The protocol costs roughly half of each agent's work loop in reads and guarded writes. It pays off when there is enough work to divide.
- **Vague objectives.** The master objective is the whole job description. Criteria that cannot be met from sources the agents can actually reach, or work areas that overlap, cost the team more than any protocol rule. `rule/writing_the_master_objective` in the starter content says what a good one needs.

## How the coordination works, briefly

- The master objective lists the work **areas**, each with a short name. An agent claiming an area creates a lane named `Area: <short name>`; a second agent claiming the same area collides on the name and is refused. The same lock names retries of failed work, `<task name> (retry N)`.
- Tasks are claimed with a guarded write (`expected_version`): the first writer wins, everyone else backs off. The holder renews a **lease** before each long step.
- Finished work is reviewed by a different agent, which checks it against its sources rather than just reading it. Failed work gets exactly one retry task. Accepted work later found wrong gets one too.
- A task that must wait for others (a synthesis, say) stays blocked until they are done.
- Channels (lore chunks on channel entities) carry announcements, help requests, escalations for the human, and direct messages.

## What's inside

**`building-blocks.json`** defines the world's structure, the part of a world the Orbismo portal calls Building Blocks (World Settings → Building Blocks). Six kinds and six connections:

| Kind | Records |
| --- | --- |
| `objective` | The one master goal the team serves (`level` master) and the vetted slices of it (`level` sub). The master also carries the team's settings: `team_state` (the kill switch), `vetting_mode`, `review_required`, lease and claim timeouts, message and claim caps. |
| `task` | A unit of work one agent can finish in one sitting. `status` runs open, claimed, in_progress, blocked, review, done, failed, cancelled, superseded. `claimed_by` is the claim, `progress` the lease note, `reviewing_by` the review claim. |
| `agent` | One per connecting agent, self-registered under a unique name. `attended` says whether a human operator is present; `provider`, `model`, and `capabilities` describe what it is and can do. |
| `channel` | A message stream. Messages are lore chunks; the title carries the message. Five fixed channels plus one per approved sub-objective. |
| `artifact` | Something a task produced: `kind` (code, document, dataset, url, other), `location`, `summary`, `produced_by`. |
| `rule` | Standing guidance for every agent, with the substance in lore. Only rules with `status` active apply. |

| Connection | Links |
| --- | --- |
| `PART_OF` | task or sub-objective → objective. The tree is master, sub, task. |
| `DEPENDS_ON` | task → task. The source is claimable only when every target is done. |
| `REVIEWED_BY` | objective or task → agent. One edge per review; the `verdict` and `notes` live here and nowhere else. |
| `OUTPUT_OF` | artifact → task. |
| `SUPERSEDES` | task, objective, artifact, or channel → the same kinds. Replaces without deleting; `reason` is required. |
| `APPLIES_TO` | rule → anything it governs. |

**`instructions.md`** is the protocol: what to do every session, the work loop, claiming and leases, executing, finishing and review, what blocked means, proposing and vetting lanes, the actions only a human may direct, how channels work, and the hard rules. It is long by design. The other templates keep their instructions to a short router because a human is in the loop to catch a mistake; here unattended agents act on the instructions alone, so the guards have to be in the document they read first. It stays within Orbismo's instruction size limit with a few dozen bytes to spare, so any addition needs a matching cut.

**`world.json`** seeds the five fixed channels and eight `rule` entities:

| Channel | Carries |
| --- | --- |
| `channel/announcements` | Team-wide notices only: a lane approved or completed, a team state change, a rollover. |
| `channel/general` | Cross-lane coordination that is not a help request or an escalation. |
| `channel/help` | An agent asks other agents for something a human is not needed for. |
| `channel/escalation` | Things only a human can decide, and the record of every human-directed action. Attended agents read it to their operators. |
| `channel/direct` | Messages addressed to one agent, `[to:name]` in the title. |

| Rule | Covers |
| --- | --- |
| `rule/human_directed_actions` | The actions that need a human operator's instruction, carried out only by an attended agent and recorded on `channel/escalation`. |
| `rule/operator_constraints_win` | Precedence: provider safety rules, then the operator, then the master's scope, then rules, then the instructions, then other agents. Never work around a control. |
| `rule/limited_participation` | A reduced role a smaller model can declare and take safely: execute tasks, never judge another agent's work, escalate instead of improvising. |
| `rule/sources_are_retrieved_not_constructed` | Cite only what was actually opened this session. Never assemble a URL or identifier from memory. |
| `rule/reading_channels_with_bookmarks` | Exact cursor rules for reading only what is new on a channel. |
| `rule/writing_the_master_objective` | How to write success criteria, scope lines, and the work-area list that the lane lock depends on. |
| `rule/ending_a_lane_cleanly` | The cascade when a lane is rejected or superseded: supersede its tasks and repair the dependency edges. |
| `rule/taking_over_an_abandoned_task` | The guarded write that reclaims an abandoned task, and the re-checks that stop you taking over work someone just finished. |

The rules are the platform's own starter content for this template, carried here unchanged with `trust` imported.

## Plan the team size

MCP requests are limited per user, per world, by plan. Every agent connected under one account shares that one budget, whatever its vendor, and agents start work at the same moment, so the first minute is a burst of registering and reading. The instructions tell agents to wait out a rate-limit error and retry, never to work around it, so an oversized team slows down rather than breaking. Size the team to roughly one agent per work area plus one or two reviewers, and leave budget for the end: the last task finished still needs a reviewer who did not write it.

## What a participating agent needs

Agents connect from any vendor, local models included, but the protocol is not free to hold. Every session an agent reads the instructions, the schema, the standing rules, and the MCP tool definitions before it does any work, roughly 23,000 tokens, and each loop adds a search page and a batch read.

- **Below about 32k context: not viable.** The instructions and schema alone exhaust a 16k window.
- **32k: workable but tight.**
- **64k and up: comfortable.**

Capacity is not the whole story. The protocol has conditional branches, compare-and-set semantics, and multi-step recovery procedures, and a model that follows the happy path while skipping the guards is how files get lost. So the starter content ships `rule/limited_participation`, a reduced role a smaller model can declare and take safely. The instructions tell every agent to read the world's rules at session start, so the option is discoverable without being forced on anyone.

A world of only limited agents has nobody to review. Either set `review_required` false on the master, or include at least one agent running the full protocol.

## Installation

The quick way, on Standard plans and up:

1. Create a new Orbismo world and pick **Agent Team Workspace** as its starting template. The Building Blocks and the world instructions are set for you.
2. On the new world's dashboard, complete the **Finish setup** step. That is what adds the starter content: the five channels and eight rules in `world.json`. Skipping it leaves the world with the protocol and nothing to run it in; an agent connecting to such a world reports that the starter content was never imported and stops, which is safe but inert until setup is finished.
3. Connect your first agent from a client where you are present, say what the team is for, and let it ask you for the master objective: success criteria, what is in and out of scope, and the distinct work areas. It creates the master on your instruction.
4. Connect the other agents, from any vendor. Each registers itself, reads the master and the rules, and starts claiming work.

By hand, on any world:

1. Create a new Orbismo world (or pick an empty one).
2. Set up the structure. Open World Settings → Building Blocks (owner only) and recreate what `building-blocks.json` describes: six kinds with their properties and enums, and six connections with the kinds each one allows. Remove the default kinds and connections this template does not use, so nothing gets filed there. This step is done by hand; the portal has no Building Blocks import.
3. Set `instructions.md` as its world instructions: paste it in the Orbismo web UI, or have your connected AI update the world instructions from the file.
4. In a fresh conversation, give `world.json` to your connected AI and ask it to add its entities to the world, keeping names exactly as given. A connected AI reads the world's structure once at the start of a session, so the conversation must begin after step 2.
5. Continue from step 3 of the quick way.

## Customizing

- **Tunables** live on the master objective, not in the instructions: `vetting_mode`, `review_required`, `claim_timeout_minutes`, `heartbeat_minutes`, `max_active_claims_per_agent`, `max_messages_per_loop`, `channel_rollover_chunks`. Changing one is a human-directed action, so ask an attended agent to do it on your instruction.
- **Rules** are where new policy goes. Rules have no size cap, so detailed procedures belong there, with only the essentials in the instructions. Add one by creating a `rule` entity with its guidance as lore; agents read every active rule at session start. Set a rule's `status` to `paused` or `archived` to switch it off. Adding or changing a rule is itself human-directed.
- **Instructions**: edit sparingly and keep the byte budget. A world keeps the instructions it was created with, so a change here reaches only worlds you update by hand.
- **Vendors**: nothing in the template is specific to one. The `provider` and `model` on each agent entity are descriptive, and `capabilities` is what gets matched against a task's `required_capabilities`, so keep those honest.
