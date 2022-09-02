# Update the schema and codelists from Airtable

Development of the Open Fibre Data Standard (OFDS) schema and codelists is managed in [Airtable](https://airtable.com/apprMa4GXD05csfkW/).

To update the schema and codelists from Airtable:

* Follow the instructions in [Get started](build.md#get-started)
* [Get your Airtable API key](https://support.airtable.com/docs/how-do-i-get-my-api-key-)
* Add your API key to an environment variable named `AIRTABLE_API_KEY`
* Run the following command:

```bash
./manage.py update-from-airtable
```

This command adds new definitions and properties, updates existing definitions and properties, and deletes definitions and properties that do not exist in Airtable or are marked as 'Omit'.
