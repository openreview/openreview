# Customizing your submission form

You can customize your venue’s submission form using the [Revision](../../reference/stages/revision.md) button on your [venue request form](navigating-your-venue-pages.md#venue-request-form). Notice that the JSON is surrounded by a single set of curly braces, as shown below:

{% code title="" %}
```json
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

If you want to limit who in the program committee can read a field in the submission, you can use the `readers` field. The example below limits the readers of the `private comments` field to just authors, Assigned Senior Area Chairs, and Program Chairs. Remember to include authors as readers of these limited fields or they won't see them.

<pre class="language-json"><code class="lang-json"><strong>"private_comments": {
</strong>  "value": {
    "param": {
      "type": "string",
      "optional": true
    }
  },
  "readers": [
    "Your/Venue/ID/Program_Chairs",
    "Your/Venue/ID/Submission${4/number}/Senior_Area_Chairs",
    "Your/Venue/ID/Submission${4/number}/Authors"
  ]
}
</code></pre>



To remove fields, enter a comma-separated list of lowercase field names in the ‘Remove Submission Options’ field. To learn more about accepted field types, refer [here](../frequently-asked-questions/what-field-types-are-supported-in-the-forms.md).&#x20;
