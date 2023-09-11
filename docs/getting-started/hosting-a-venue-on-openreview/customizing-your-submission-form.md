# Customizing your submission form

You can customize your venue’s submission form using the [Revision](../../reference/stages/revision.md) button on your [venue request form](navigating-your-venue-pages.md#venue-request-form). New fields can be entered in different JSON formats depending on the API version your venue is using. Notice that the JSON is surrounded by a single set of curly braces, as shown below:

{% code title="API 2" %}
```
{
  "confirmation": {
    "description": "Please confirm you have read the workshop's policies.",
    "order": 2,
    "value": {
      "param": {
        "type": "string",
        "enum": [
          "I have read and agree with the workshop's policy on behalf of myself and my co-authors."
        ],
        "input": "radio"
      }
    }
  }
}
```
{% endcode %}

{% code title="API 1" %}
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
{% endcode %}

To remove fields, enter a comma-separated list of lowercase field names in the ‘Remove Submission Options’ field. To learn more about accepted field types, refer [here](../frequently-asked-questions/what-field-types-are-supported-in-the-forms.md).&#x20;
