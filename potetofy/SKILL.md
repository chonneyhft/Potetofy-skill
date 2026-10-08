---
name: potetofy
description: >-
  Use when asked to audit or redesign a bot team, agent org, or single agent
  workflow the way poteto would (/potetofy). Read-only. Returns findings and an
  approval-gated before/after plan.
---
# Potetofy

Audit an agent setup the way poteto would, then return a before/after plan. The method and its sources are in `reference.md` next to this file. Read it before judging anything.

## Boundaries

- Read only. Don't change bots, skills, routines, models, repos or PRs, and don't message the bots being audited.
- The plan is the deliverable. Apply items only in a later turn, only the ones the user picks, one at a time.

## 1. Scope

Name the target: a whole agent org, or one workflow from trigger to finished result. Default to the last 7 days. If the target is unclear, ask one question. Note recent changes so you don't re-recommend them, but check they actually landed.

## 2. Gather evidence

Build a picture from real artifacts, not from what the bots say about themselves:

- each agent's job, instructions, tools and skills
- who talks to whom, and how often
- routines and how often they find nothing new
- cost signals: messages, wakes, computer-use steps, cloud agent runs and their model settings
- how verification works and what it's tied to
- repeated corrections from the human or reviewers
- where the standing rules actually live

Trace a few real work items end to end: every handoff, every ask to the human, and who verified what. Label numbers as measured, inferred or guessed. Mark anything you couldn't get as missing.

## 3. Judge against the method

Look at each area below and decide whether it holds. Cite evidence for every gap.

1. **Trust before scale.** Has each workflow earned its level of autonomy (watched, then skill, then one-shot, then routine or auto-merge)? Was parallelism added before verification could support it?
2. **One job per agent.** Does each agent have one clear job and explicit anti-jobs? Mixed context, unused tools, or a bot that only relays another bot's output suggests a split, a trim or a merge.
3. **Managers delegate.** Do coordinating agents stay out of leaf work and hand it to cloud agents or subagents with clear ownership?
4. **Independent checks on the real thing.** Is every change verified against the running product, by someone other than its author, with evidence tied to the current version? Is work that gets checked by hand every time scripted yet?
5. **Corrections become structure.** For each repeated mistake, is the fix enforced as high as it can be: structure, then lint/CI/scripts, then rules or reviewers, then skills, then prose last?
6. **Goals, not scripts.** Do briefs give a goal, scope, a pass/fail done check and a report shape, with the standing rules attached?
7. **Human gates.** Is the human asked only about irreversible actions and real product calls, with those asks batched?
8. **Cost discipline.** Do routine intervals match how often things change? Are expensive model settings justified? Are scripts used instead of swarms? Do summaries flow up instead of raw output? Is the overhead sized to the task?

## 4. Write the plan

**Findings**, most impact first:

| Area | Current state (evidence) | Poteto way | Change | Impact | Risk |
|---|---|---|---|---|---|

**Changes**, in two groups:

- **Do now (reversible).** For each one: before and after, who owns it, how you'll know it worked, and how to roll it back. Keep this list short, and prefer removing a rule or tool to adding one.
- **Needs your call.** Product calls, anything irreversible or costly, changes to who can merge or ship, and adding or retiring agents. One line each: the question, the options, and your default.

Rules for the plan: never lower the quality bar (when you cut cost, name the check that still guards quality), label inferences, and size the redesign to the team.

## 5. Stop

Present the findings and changes, then end with: "Nothing has changed. Tell me which items to apply."
