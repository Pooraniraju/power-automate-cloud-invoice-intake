# Setup guide

## 1. Dataverse tables

- `cr_configsenders` — sender-domain routing config (see [`../dataverse/config-senders-table-schema.json`](../dataverse/config-senders-table-schema.json)).
- `cr_invoiceworkqueueitem` — work queue (see [`../dataverse/work-queue-table-schema.json`](../dataverse/work-queue-table-schema.json)).

## 2. Connections

Office 365 Outlook (shared mailbox), Microsoft Dataverse, Excel Online (Business), AI Builder, Microsoft Teams — create each in your target environment.

## 3. AI Builder model

Publish the prebuilt **Invoice Processing** model, or point the flow at a custom-trained model if your invoices need it.

## 4. Import the flow

See [`../flow-definition/README.md`](../flow-definition/README.md).

## 5. Test both paths

- Send a `.xlsx` attachment from a sender domain in `cr_configsenders` → confirm a `cr_invoiceworkqueueitem` row lands with `DocumentType = Excel`, `Status = Completed`.
- Send a `.pdf` invoice → confirm AI Builder extraction runs and the row lands as `Completed` (high confidence, totals match) or `Needs Review` (low confidence or mismatch) with a Teams adaptive card sent to `cr_defaultowner`.
- Re-send the same email (or re-trigger on the same message ID) and confirm no duplicate row is created — this is the `cr_sourceemailid` idempotency check.
