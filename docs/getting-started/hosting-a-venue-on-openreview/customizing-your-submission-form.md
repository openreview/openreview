# Customizing your submission form

You can customize your venue’s submission form using the [Revision](../../reference/stages/revision.md) button on your [venue request form](navigating-your-venue-pages.md#venue-request-form). New fields can be entered in JSON format, surrounded by a single set of curly braces, as shown below:&#x20;

```
{
    "confirmation": {
      "description": "Please confirm you have read the workshop's policies.",
      "value-radio": [
          "I have read and agree with the workshop's policy on behalf of myself and my co-authors."
      ],
      "order": 2,
      "required": true
    }
}
```

To remove fields, enter a comma-separated list of lowercase field names in the ‘Remove Submission Options’ field. To learn more about accepted field types, refer [here](broken-reference).&#x20;
