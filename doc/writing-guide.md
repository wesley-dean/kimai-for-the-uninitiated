# Writing guide

## Write for the uninitiated

Assume that the reader can operate a web application but does not yet know how Kimai thinks about work.  Introduce product vocabulary before relying on it.

Do not write a second reference manual.  The official Kimai documentation already serves that purpose.

## Explain the decision before the interface

Prefer this order:

1. What problem does this feature solve?
2. Does the reader need it now?
3. What decision must the reader make?
4. What are the consequences of that decision?
5. How is it configured in the web interface?
6. How can the reader check that it worked?
7. Where is the authoritative reference documentation?

A page should not begin with a tour of fields unless understanding those fields is itself the reader's problem.

## Distinguish behavior, recommendations, and examples

Use clear language when a statement is a project recommendation rather than a Kimai requirement.  Phrases such as "For a small installation, a useful starting point is..." or "You can usually leave this alone until..." are appropriate when they accurately describe the recommendation.

Worked examples should use fictional organizations and people.  Keep recurring examples consistent across pages where doing so helps the reader build on earlier knowledge.

## Prefer smaller starting configurations

A feature is not a prerequisite merely because it exists.  Explicitly tell readers when they can postpone teams, custom roles, detailed tags, budgets, custom fields, or other optional machinery.

## Preserve history

Kimai is a time-tracking and invoicing system, so historical records matter.  When current Kimai documentation warns that deleting an object removes linked data or otherwise has accounting consequences, favor reversible lifecycle actions such as hiding, deactivating, or canceling where appropriate.

## UI instructions

Use durable breadcrumbs such as **System > Users** when the current UI provides a stable path.  Avoid screenshots when text communicates the same information more durably.

Use screenshots only when layout, grouping, or visual state materially helps the reader.  Do not use a screenshot merely to prove that a button exists.

## Upstream sources

Prefer official Kimai documentation as the factual source for product behavior.  Link to the relevant upstream page near the material it supports or in a clearly identified reference section.

Do not copy substantial passages from upstream documentation.  Summarize and explain concepts in original language.

## Version drift

Kimai changes over time.  When a page depends on a specific UI path or behavior, verify it against a current version before making a confident claim.

If upstream behavior changes, update both the instruction and the reasoning around it.  A stale explanation can be more misleading than a stale button label.
