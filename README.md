# Property-Based Testing Skill

[![Download Property-Based Testing Skill](https://img.shields.io/badge/Download-property--testing.skill-blue?style=for-the-badge)](https://github.com/nyxandro/property-testing-skill/raw/main/dist/property-testing.skill)

A property-based testing skill for Claude Code and OpenCode: fast-check in TypeScript
(Vitest) and Hypothesis in Python (pytest). An example test checks the inputs someone thought
of; a property test states what must hold for every input, lets the library generate hundreds
of inputs, and shrinks the one that breaks the statement to the smallest case. The first real
user of a product types three tax IDs into a field written for one; this is the kind of test
that finds it before they do.

The skill is a method, not a tutorial. It assumes the agent knows the libraries and fixes the
decisions that go wrong when property tests are written quickly: which property to state and
how strong it is, how to describe the input honestly, how to prove the test can fail, what a
green run does and does not mean, and when to stop.

## What the agent does

1. Decides from the shape of the function whether a property test is worth it, and which
   property to state: round-trip, idempotence, invariant, order independence, metamorphic
   relation, cheap verification, oracle, determinism, or at the very least "never crashes,
   rejects with the project's own error". Asserts the strongest one the code supports.
2. Refuses two kinds of empty tests: tautologies that restate the implementation, and vacuous
   tests whose preconditions discard nearly every input.
3. Proves the property can fail by planting a bug, seeing red, and reverting.
4. Builds the input by construction, not filtering. Generates from the boundary, not from the
   type: a free-text field carries any string, a webhook carries any JSON. Pins known
   boundaries and every production incident as permanent examples. Generates money as integer
   minor units. Keeps one shared generator per domain type.
5. Treats streams and events as their own case: any arrival order gives the same state,
   duplicates change nothing, a snapshot plus deltas equals the full history. Uses the
   fast-check scheduler for asynchronous order. Says plainly that races between processes and
   containers are not property-testable and names what covers them instead.
6. Tests stateful systems against a deliberately simple model.
7. Keeps proportion: one property per function with a clear property, example tests stay,
   default run counts, seed never fixed permanently. Adding the library to a project that lacks
   one is offered once as the owner's decision.

Three situations have their own reference, read only when the situation arises: the code seems
to have no property to assert (six rearrangements that expose one), a property test failed
(wrong property, ambiguous spec or real bug, and how to tell), and a review of existing
property tests that may assert nothing.

## Install

**Claude Code, as a plugin**, from [nyxandro/skills](https://github.com/nyxandro/skills), the
catalog of my skills:

```text
/plugin install property-testing --marketplace nyxandro/skills
```

Use one way or the other: with both, Claude Code loads the skill twice.

**Any agent, as a skill folder:**

Download `dist/property-testing.skill` (the badge above) and unpack it into the skills folder
of your agent, or clone this repository and link the `property-testing/` folder there. No
scripts, no build step.

## Repository layout

```text
property-testing/
  SKILL.md                              the method and the tooling for TypeScript and Python
  references/expose-a-property.md       when the code seems to have no property
  references/triage-a-failure.md        when a property test failed
  references/review-existing-tests.md   when reviewing existing property tests
dist/property-testing.skill             downloadable archive of the skill folder
```

## License

MIT.
