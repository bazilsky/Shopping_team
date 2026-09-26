# Shopping team

Given any product and the specifications and attributes you lock in a brief, produce a sourced shortlist and one purchase recommendation. Retrieval must not be whatever one search path ranked first, and soft preferences must not steer the queries. You alone spend, hold accounts, and check out.

## Agents

| Agent | Cursor name | Role |
| --- | --- | --- |
| Shopper | `shopping--shopper` | From the hard-constraint packet only, retrieve candidates on the assigned route and emit a structured table. |
| Fit auditor | `shopping--fit-auditor` | Accept or reject the named pick against the brief, using pages it fetches itself. |

Union, hard-constraint filtering, sponsored labeling, and soft-preference sort are a script. The note that explains the winning row is a template filled from that row. Neither is an agent.

## Delegation rules

Two Shopper invocations run when a marketplace allowlist and a manufacturer-or-independent allowlist can be enforced. That dual search is the default for this team. When the allowlists cannot be enforced, one Shopper pass covers both routes and the run says so.

Each invocation receives the hard-constraint packet only. Neither sees the other's table, queries, or the recommendation. The script owns order. Sponsored or affiliate rows stay in the table and sort after an unsponsored row that ties on your soft preferences. The template does not re-rank.

On reject, the script advances one eligible row and the auditor checks that row once. The loop ends there.

Stop for you before a confident recommendation when a hard constraint is ambiguous, or when the product is regulated or safety-critical. Price and stock come from a fetch at table-write time and again at audit time.

## Human checkpoints

- Locking or changing the brief.
- Any hard constraint that is still ambiguous.
- Regulated or safety-critical goods, before a confident recommendation.
- Spend, accounts, and checkout.
- A second reject, a single-source label, or a near-tie you want to override.
