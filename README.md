# Agent Entitlement Loop

**Live results page:** https://eshwarpotturi.github.io/agent-entitlement-loop/
**Architecture brief:** https://eshwarpotturi.github.io/agent-entitlement-architecture/

## What it is
A proof of concept where AI agents set and harden their own permissions in a (synthetic) bank:

1. A **designer agent** drafts a least-privilege rulebook from each system's tool list and each agent's role.
2. A **red-team agent** tries to make the business agents break bank rules (leak confidential deal data, tamper with the restricted list, pitch a restricted stock).
3. A **breach check** (plain code) proves each breach from system state, never from what an agent claims.
4. The designer **revises the rulebook**; business tasks are re-run so any capability cost is visible.
5. The final rulebook is tested against **attacks the designer never saw**.

An **entitlement gateway** (plain code, no LLM) enforces the rulebook on every tool call: allow, deny, or hold for human approval.

## Why
Banks are deploying agents across many systems, but there is no agreed way to decide which agent may do what, on whose behalf. Hand-writing rules for every agent and system does not scale, and static rules miss attacks that use legitimate access (for example one agent asking another to fetch data for it).

## Result of the recorded run (13 Oct 2026, GPT-4.1-mini via LLM Foundry, 156 calls, about $0.04)
| Round | Rulebook | Attack goals breached | Business tasks working |
|---|---|---|---|
| 0 | none | 4 of 4 | 5 of 5 |
| 1 | designer's first draft | 1 of 4 (confused deputy: Sales got deal data via the Banking agent) | 5 of 5 |
| 2 | designer's fix: an agent acting for another gets only the rights both have | 0 of 4 | 5 of 5 |
| Unseen | final rulebook vs 2 new goals | 0 of 2 | – |

## Files
- `index.html` – results page (rulebook per round, goals breached, full attack log)
- `engine.js` – the loop as run in the browser against LLM Foundry
- `bank.py`, `run.py` – the same loop in Python (`python run.py --scripted` runs offline with no LLM)

## Limits
Synthetic data, fictional companies, 2 agents and 4 systems. It demonstrates the method, not coverage of a real estate of systems. An AI red team adds to human penetration testing; it does not replace it.
