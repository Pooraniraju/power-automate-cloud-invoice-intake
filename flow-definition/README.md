# Flow definition (reference)

`invoice-intake-flow.json` is a reference Workflow Definition Language export in the shape Power Automate produces on export, covering the path described in the main README: mailbox trigger → sender lookup → per-attachment branch by file type → Excel parsing or AI Builder extraction → work-queue write, with the confidence/total-mismatch check routing to a Teams adaptive card for review.

As with the other repos in this profile, connection references, the mailbox address, and Dataverse table logical names are illustrative placeholders — see the main README's note on sample data.

## To stand this up for real

1. Create the two Dataverse tables in [`../dataverse`](../dataverse) and load the `cr_configsenders` sample rows.
2. Create connections: Office 365 Outlook (shared mailbox), Dataverse, Excel Online (Business), AI Builder, Microsoft Teams.
3. Publish the AI Builder **Invoice Processing** prebuilt model (or a custom-trained one) in your environment.
4. Import the JSON through **Power Automate → My flows → Import → Import Package**, or paste into the flow's code view and remap connections.
5. Send a test Excel and a test PDF invoice to the watched mailbox from a sender domain that exists in `cr_configsenders`, and confirm both paths land the expected `cr_invoiceworkqueueitem` row.
