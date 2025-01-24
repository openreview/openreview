# Customizing your submission form

For an overview and basics of form customization, see the comprehensive [Customizing Forms](../customizing-forms.md).&#x20;

You can customize the [default submission form](../../reference/default-forms/default-submission-form.md) for your venue using the  [Revision](../../reference/stages/revision.md) button on your [venue request form](navigating-your-venue-pages.md#venue-request-form).  In the 'Additional Submission Options', field, enter valid JSON with the fields that you would like to add or change in your form. Shown below is an example of adding a field for authors to nominate a reviewer from the author list of their paper.

{% tabs %}
{% tab title="Website" %}
<figure><img src="../../.gitbook/assets/Screenshot 2024-08-22 at 12.01.38 PM.png" alt=""><figcaption></figcaption></figure>


{% endtab %}

{% tab title="JSON" %}
```json
{
   "serve_as_reviewer": {
      "value": {
          "param": {
            "type": "profile[]",
            "regex": "~.*"
          }
      },
      "description": "Please nominate an author to serve as a reviewer using their profile ID (e.g. ~First_Last1)",
      "order": 20
   }
}
```
{% endtab %}
{% endtabs %}

The resulting field in the submission form would look like this:

<figure><img src="../../.gitbook/assets/Screenshot 2024-08-20 at 10.59.52 AM.png" alt=""><figcaption></figcaption></figure>

## Common Customizations

### Asking authors to agree to conference policies

<figure><img src="../../.gitbook/assets/Screenshot 2024-08-20 at 11.13.02 AM.png" alt=""><figcaption></figcaption></figure>

<pre class="language-json" data-title=""><code class="lang-json"><strong>{
</strong>  "confirmation": {
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
</code></pre>

### Limit read-permissions for certain fields

The `readers` field can be used to list who will be allowed to read a specific field of the submission form. The example below limits the readers of the `private comments` field to just authors, Assigned Senior Area Chairs, and Program Chairs.&#x20;

Note: Authors will not be able to read these fields if they are not in the readers list

<figure><img src="../../.gitbook/assets/Screenshot 2024-08-20 at 11.17.59 AM.png" alt=""><figcaption></figcaption></figure>

```json
{
  "private_comments": {
    "value": {
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
}
```



### Adding tracks to your venue

Once you have [reviewed our support for "tracks" in a single venue](../../how-to-guides/workflow/how-to-have-different-tracks-or-types-of-submissions-for-a-single-venue.md) and you think this is what your venue needs, you can add a "track" field to your submission form if you are using separate reviewing pools for track submissions.

<figure><img src="../../.gitbook/assets/Screenshot 2024-08-20 at 11.23.48 AM.png" alt=""><figcaption></figcaption></figure>

```json
{
  "track": {
    "description": "Please select the track you are submitting to.",
    "order": 2,
    "value": {
      "param": {
        "type": "string",
        "enum": [
          "Track 1",
          "Track 2",
          "Track 3"
        ],
        "input": "radio"
      }
    }
  }
}
```

