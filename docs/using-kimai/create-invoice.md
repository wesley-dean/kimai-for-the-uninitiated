# Create and manage invoices

Kimai creates invoices from recorded, billable time.  For a newcomer, the most useful way to think about invoice creation is as the point where reviewed time becomes processed billing history.

```mermaid
flowchart LR
    R[Reviewed billable time] --> I[Create invoice]
    I --> N[New]
    N --> W[Waiting for payment]
    W --> P[Invoice paid]
    N --> C[Canceled]
    W --> C
```

## Prepare invoicing once

Before creating a real invoice, make sure the administrative pieces exist.

### Create your own organization as a customer

Kimai uses a customer record as the **invoice issuer**.  That record represents the organization sending the invoice.  Fill in the company and address information you expect to appear on invoices.

This is separate from the customer receiving the invoice.

### Check the receiving customer

The customer's currency is used for time-entry prices and invoices.  Kimai's invoice module operates on one customer at a time, which also avoids combining multiple customer currencies into one invoice.

Check the customer's company name, address, contact information, currency, and other invoice-relevant fields before generating a real document.

### Create an invoice template

Open **Invoices > Templates** and create an invoice template.  An invoice template is not merely visual styling; it also carries billing behavior and metadata.

For a basic first template, pay attention to:

- internal template name;
- invoice title;
- invoice issuer;
- terms of payment;
- contact and bank/payment text when applicable;
- tax rate;
- payment term in days;
- language;
- invoice-number generator;
- document template/renderer;
- grouping of invoice lines.

Kimai can associate a default invoice template with a customer, which makes repeated billing less error-prone after you have validated the template.

## Review the time before opening the invoice flow

Do not use invoice creation as the place where you discover incorrect time records.  Complete the [pre-invoice review](review-time.md) first.

Only billable items are included in invoices.  If time is unexpectedly absent, check its date range, customer/project/activity classification, billable state, and whether it was already exported or invoiced.

## Why "Create invoice" opens a search page

This is expected behavior in current Kimai.

Kimai does **not** begin invoice creation with a blank invoice form.  The **Invoices > Create invoice** page is first a search/filter screen that asks Kimai which existing billable records should be considered for invoicing.

```mermaid
flowchart LR
    A[Invoices > Create invoice] --> F[Filter eligible records]
    F --> S[Search]
    S --> R{Eligible records found?}
    R -->|No| N[Nothing to invoice]
    R -->|Yes| P[Customer preview rows]
    P --> V[Preview invoice]
    P --> C[Create/save invoice]
```

The first screen therefore contains filters such as billing period, customer, project, activity, tags, users, teams, and export state.  It is selecting source records, not editing an invoice document.

This distinction is easy to miss because the menu says **Create invoice**, while the first page looks like a search page.

## Create the invoice through the web interface

For a typical customer invoice:

1. Make sure the billable source records already exist.  For hourly work, those are ordinary billable time entries.  For [fixed-price fees](fixed-price-fees.md), those are the billable fixed-price records that represent the units you intend to charge.
2. Open **Invoices > Create invoice**.
3. Set the billing period that contains the records you want to invoice.
4. Select the customer receiving the invoice.
5. Narrow the result by project, activity, tags, user, or other filters if you do not want every eligible record for that customer and period.
6. Leave the export-state filter at its normal unprocessed setting unless you deliberately need previously processed records.
7. Click **Search**.
8. Kimai then builds one or more customer preview rows from the eligible records it found.
9. Expand a customer row if you want to inspect the underlying entries and confirm their quantities, rates, durations, and totals.
10. Use the **Preview** action to inspect the rendered invoice before committing it.
11. Confirm the invoice template and invoice date for that customer if the interface presents those controls.
12. Use the invoice/save action on the customer row to create the invoice.

Creating the invoice is the final step in this flow, not the first.

### If no create button appears

Kimai only renders the customer preview and create actions when the search produces an invoice model containing eligible records.

If the search returns nothing, check these prerequisites:

- an invoice template exists;
- the source time/fixed-price records already exist;
- the records belong to the intended customer through their project;
- the records fall inside the selected date range;
- the records are billable;
- the customer, project, and activity are not preventing automatic billability;
- the records have not already been invoiced/exported, unless you intentionally search for processed records;
- your user has permission to create invoices and access the relevant customer data.

Stock Kimai does not use this screen to create an empty manual invoice with arbitrary lines.  It creates the invoice from the records returned by this search.

### Example: three fixed-price seats

Suppose you have already recorded three fixed-price units for a customer:

| Record | Activity | Fixed price |
| --- | --- | ---: |
| 1 | Google Workspace seat | $25 |
| 2 | Google Workspace seat | $25 |
| 3 | Google Workspace seat | $25 |

To invoice those records:

1. Open **Invoices > Create invoice**.
2. Select the customer.
3. Choose a date range containing all three records.
4. Optionally filter to the **Google Workspace seat** activity.
5. Click **Search**.
6. Confirm that Kimai finds the three records and that the preview total is $75.
7. If your invoice template groups lines by activity, verify that the rendered result represents the three matching units as intended.
8. Preview the invoice.
9. Create it only after the source records and total are correct.

The exact fields shown can vary with configuration and installed Kimai version, but the underlying rule is stable: the invoice is built from the eligible records selected by the invoice query, and only billable items qualify.

## What creation changes

Creating an invoice is a meaningful state transition for the time records involved.

Kimai uses the export flag to mark those records as processed.  Processed records are excluded by default from future invoice/export selections and regular users cannot edit them.

```mermaid
stateDiagram-v2
    [*] --> Recorded
    Recorded --> Reviewed
    Reviewed --> Processed: invoice or export
    Processed --> Processed: normal historical state
```

That behavior helps prevent the same ordinary time from being casually invoiced twice, but it is also why mistakes should be caught before invoice creation.

## Understand invoice states

Kimai currently defines these invoice states:

| State | Meaning |
| --- | --- |
| New | The invoice was created |
| Waiting for payment | The invoice was sent to the customer |
| Invoice paid | Payment was received; Kimai requires a payment date |
| Canceled | The invoice was invalidated or needs replacement |

The state is useful operational history.  A freshly generated invoice does not become "paid" merely because the document exists.

## Prefer cancellation over deletion

Kimai explicitly warns against deleting invoices.  Depending on the invoice-number format, deletion can interfere with counters and may allow a future invoice number to collide with one that was already used.

For an invoice that should no longer be valid, prefer **Canceled**.  The canceled invoice remains in history and retains its invoice number.

This is one of the places where preserving history is more important than making the screen look tidy.

## Cancellation does not make the original time ordinary again

There is an important subtlety here.

Canceling an invoice does **not** reset the export flag on the timesheets that were processed when the invoice was created.  Kimai also documents that it does not retain a direct list of which items belonged to a canceled invoice for purposes of automatically rebuilding it.

If you need to create a replacement invoice using the same time, you may need to include already-exported records in the relevant filtering process and carefully reconstruct the intended set.

```mermaid
flowchart TB
    I[Invoice created] --> X[Time marked processed]
    I --> C[Invoice canceled]
    C --> K[Invoice number remains in history]
    C --> X
    X --> R[Replacement requires deliberate selection of processed time]
```

For that reason, cancellation is safer than deletion for invoice history, but cancellation is not an "undo" button for the underlying time records.

## A first-invoice rehearsal

Before creating the first real customer invoice, a useful rehearsal is:

1. Create a few disposable test time entries for a test or internal context.
2. Confirm their rates and billable state.
3. Search for them from **Invoices > Create invoice**.
4. Preview the generated invoice using the intended template.
5. Inspect the generated document carefully.
6. Create the invoice only after the preview is correct.
7. Confirm the invoice appears in **Invoice history**.
8. Confirm the source time is now treated as processed.

Be deliberate about invoice numbers while testing.  Once real invoice numbering begins, treat generated invoices as accounting history and prefer cancellation over deletion.  The official initial-setup page suggests deleting a test invoice, while the current invoice reference warns generally against deletion because of numbering consequences; this guide follows the more conservative rule once production numbering matters.

## After sending and payment

When you actually send the invoice, move it to **Waiting for payment** if that state matches your workflow.  When payment arrives, mark it **Invoice paid** and record the payment date.

The **Invoice history** view can then be filtered by creation date, customer, and state, giving you a lightweight view of the invoice lifecycle.

## Official reference

See Kimai's [Invoices](https://www.kimai.org/documentation/invoices.html), [Customer](https://www.kimai.org/documentation/customer.html), and [Initial setup](https://www.kimai.org/documentation/initial-setup.html) documentation for the authoritative product behavior.
