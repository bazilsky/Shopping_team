# Mission

Given any product and the specifications and attributes you lock in a brief, produce a sourced shortlist and one purchase recommendation. Retrieval must not be whatever one search path ranked first, and soft preferences must not steer the queries. You alone spend, hold accounts, and check out.

# Why agents?

**Chosen: one agent plus one reviewer.** The Shopper may run twice, in two cold contexts, when a marketplace allowlist and a manufacturer-or-independent allowlist can both be enforced.

| Option | Why it lost or won |
| --- | --- |
| No agent system | Matching a brief to live pages takes judgment and tool use. A search box does not separate must-haves from preferences. |
| One capable agent | The same context that retrieves should not also certify the pick. A wrong SKU costs money, and the miss is checkable against a page. |
| **Shopper + Fit auditor** | Search and independent rejection are different jobs. A second search is a second cold invocation of the Shopper, because it is the same job on a different source pool. |
| Small specialist team | A named Blind Retriever, a parser, or a comparer would split one brief and one table. The second cold context is the separation that reduces anchoring. |
| Larger hierarchy | Standing category buyers are job titles. Non-transferable constraints stop for you. |

Retrieval is probabilistic: the same brief can yield a different table on another run. The union and rank script is deterministic given the same rows and the same locked brief.

# Architecture

```text
You lock the brief
        |
        v
  Can the two routes be enforced?
     /                         \
   yes                          no
    |                            |
Shopper context A           Shopper, one pass
marketplace only            both routes
hard constraints only       hard constraints only
    |
Shopper context B
manufacturer and
independent sources only
hard constraints only
    |                            |
    +---------- script ----------+
         union on exact identity
         drop hard-constraint failures
         label sponsored rows
         sort by soft preferences
         sponsored loses a tie
         names one pick and near-ties
              |
              v
        template for that row
        list origin and source label
              |
              v
         Fit auditor
         re-fetch
         two source classes, or single-source label
         accept or reject
              |
         reject -> next eligible row, audited once
              |
              v
            You buy
```

The two Shopper contexts do not see each other. Neither sees soft preferences.

# Agents

## Shopper

- **Name:** Shopper
- **Mission:** From the hard-constraint packet only, retrieve candidates on the assigned route and emit a structured table.
- **Responsibilities:** Search inside the route allowlist. Record brand, model, GTIN or MPN when they exist, price, availability, claimed attributes against hard constraints, source URL, source class (marketplace, manufacturer, or independent), and whether the page presents the row as sponsored or affiliate.
- **Explicit non-responsibilities:** Read soft preferences. Rank across routes. Write the case for the pick. Accept or reject. Message the other search. Spend, check out, or touch accounts. Invent a missing spec. Certify safety or regulatory compliance.
- **Inputs:** Hard constraints, exclusions, budget, region, and quantity, plus the route allowlist for this invocation. Soft preferences stay on the locked brief and are withheld.
- **Outputs:** Table A, table B, or one table when the routes cannot be separated. The Shopper owns the table it wrote.
- **Tools or access:** Web search and page fetch inside the allowlist.
- **Persistent or temporary:** Persistent role. Up to two cold invocations per brief.
- **Who invokes it:** The front door, after you lock the brief.
- **Who reviews it:** The Fit auditor reviews the script's pick, not the authorship of the raw table.

## Fit auditor

- **Name:** Fit auditor
- **Mission:** Accept or reject the named pick against the brief, using pages it fetches itself.
- **Responsibilities:** Re-fetch the cited URLs. Check each hard constraint on the pick as met, unmet, or unknown. Accept a hard-attribute claim when a marketplace page and a manufacturer or independent page agree. When only one source class exists, label the pick single-source and still accept or reject the claims that the available page supports. On reject, check the script's next eligible row once.
- **Explicit non-responsibilities:** Search for a different product. Re-rank. Judge near-ties. Merge fuzzy titles. Rewrite soft preferences. Spend.
- **Inputs:** The full locked brief, the one row under review, and its citations.
- **Outputs:** Accept or reject, the failed claim and URL when rejected, and a source label of corroborated or single-source. The auditor owns only that verdict.
- **Tools or access:** Fetch of the cited pages, including a fresh price and stock read.
- **Persistent or temporary:** Persistent. One or two passes per purchase.
- **Who invokes it:** The front door, after the script names a pick.
- **Who reviews it:** You.

Union, hard-constraint filtering, sponsored labeling, and soft-preference sort are a script. The note that explains the winning row is a template filled from that row. Neither is an agent.

# Delegation rules

Two Shopper invocations run when a marketplace allowlist and a manufacturer-or-independent allowlist can be enforced. That dual search is the default for this team. When the allowlists cannot be enforced, one Shopper pass covers both routes and the run says so.

Each invocation receives the hard-constraint packet only. Neither sees the other's table, queries, or the recommendation. The script owns order. Sponsored or affiliate rows stay in the table and sort after an unsponsored row that ties on your soft preferences. The template does not re-rank.

On reject, the script advances one eligible row and the auditor checks that row once. The loop ends there.

Stop for you before a confident recommendation when a hard constraint is ambiguous, or when the product is regulated or safety-critical. Price and stock come from a fetch at table-write time and again at audit time.

# Context architecture

**Shared:** the locked brief, one document. It holds hard constraints, exclusions, budget, region, quantity or an explicit unspecified, and ordered or weighted soft preferences. Preference weights live only there.

**Specialist:** each Shopper invocation gets the hard-constraint packet and its route allowlist. It does not get soft preferences. The auditor gets the full brief and the single row under review. The template gets the full brief, the winning row, its list origin, its sponsored flag, the near-tie rows, and the source label.

**Temporary artefacts for the run:** `brief.lock`, `candidates_A` and `candidates_B` (or one table), `union` with scores, origins, sponsored flags, and near-ties, `pick`, the recommendation template, and `audit` with the verdict, re-fetch notes, and source label.

**Decision record:** one line after you choose: the pick, the verdict, the source label, and what you did. It does not gate the loop.

**Deliberately absent:** a chat between the searches, a standing Blind Retriever file, category playbooks, cross-run memory of preferred brands, per-retailer memory, payment or account records, and live Cursor subagents.

# Workflow

1. You lock the brief, with hard constraints separated from soft preferences.
2. If the routes can be separated, two Shopper contexts search in parallel on hard constraints only. If they cannot, one Shopper covers both routes on hard constraints only.
3. The script unions exact identity matches on GTIN, MPN, or brand plus model. Fuzzy title matches stay two rows. It drops hard-constraint failures, labels sponsored rows, sorts survivors by your soft preferences, and places a sponsored row after an unsponsored tie.
4. The template states the pick, which search it came from, whether it is sponsored, the near-ties, and the list origin of each.
5. The auditor re-fetches, applies the two-source rule, and accepts or rejects.
6. A reject advances one row, audited once.
7. You buy, change the brief, or stop.

# Critique / verification loops

One accept or reject, then at most one replacement row. The searches do not critique each other. Two lists are not proof the pick is unbiased: both can still surface the same popular product, and a cited page can still be wrong. Agreement on a bad page is not confirmation.

The auditor checks hard attributes against the live pages. A hard-attribute claim is corroborated only when two source classes agree. Otherwise the pick is labeled single-source before you decide.

# Human checkpoints

- Locking or changing the brief.
- Any hard constraint that is still ambiguous.
- Regulated or safety-critical goods, before a confident recommendation.
- Spend, accounts, and checkout.
- A second reject, a single-source label, or a near-tie you want to override.

# Failure modes

1. **Routes cannot be enforced.** Both invocations hit the same index. Dual search collapses to one Shopper pass, and the run says so. Same-index overlap returns.
2. **Brief encoding.** A preference written as a hard constraint steers retrieval, because the Shopper does see hard constraints. You confirm the split before search.
3. **Sponsored ranking.** An unlabeled ad sorts as an ordinary row. The script depends on the page disclosing sponsorship.
4. **False spec on the page.** Two source classes can still repeat the same false number. The label then says corroborated, and the claim can still be wrong.
5. **Identity match.** A wrong GTIN merge, or one product left as two rows because the titles differed.
6. **Shared model prior.** Both invocations prefer widely documented products. A second model family is a later change only if runs show the same SKUs even with allowlists on.
7. **Cost.** Dual search roughly doubles retrieval.

# Simplification test

The Fit auditor stays. Accept or reject sits outside the search. A named Blind Retriever goes: the second cold Shopper invocation is the same job. That second invocation also goes when the routes cannot be separated. The ranking script stays whenever there is a multi-row table, so the model does not quietly re-order the rows. Withholding soft preferences from retrieval stays. Putting them back into the Shopper prompt lets "prefer Sony" become the query.

# Minimal architecture

One Shopper pass over both routes, still limited to hard constraints, plus the Fit auditor. The script still sorts, still labels sponsored rows, and the auditor still applies the two-source rule. Weaker on anchoring. This is the fallback when route allowlists cannot be enforced.

# High-intelligence architecture

A named second retriever, a second model family on every run, category specialists, a conversation between lists, and a model judging near-ties. Rejected for the default. Same job twice, extra hand-offs, and agreement that can still be wrong. A second model family remains a later option if enforced routes still return the same SKUs.

# Recommended architecture

Shopper plus Fit auditor. Dual cold search is the default when route allowlists can be enforced; one pass is the fallback. Each search sees hard constraints only. The script unions, drops hard failures, labels sponsored rows, and sorts by the soft preferences on the locked brief. The template reports list origin, sponsorship, and near-ties. The auditor re-fetches and either corroborates hard attributes across two source classes or labels the pick single-source. You spend.

# Implementation plan

This file is the stored design. Two standing roles do not get a split README, AGENTS, WORKFLOW, or CONTEXT. Live Cursor subagents are not installed. Installing them is a separate request; the names would be `shopping--shopper` and `shopping--fit-auditor`.
