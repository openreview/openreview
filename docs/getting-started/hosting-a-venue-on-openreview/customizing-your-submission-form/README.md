# Customizing your submission form

For an overview and basics of form customization, see the comprehensive [Customizing Forms](../../customizing-forms.md).

To make changes to your submission form, navigate to your Workflow Timeline. Then go to the "Submission" Step — > Edit Fields — > Widget. This will allow you to edit the fields in your submission form.

{% hint style="info" %}
To hide a new field from reviewers, go to "Submission Change Before Bidding" and/or "Submission Change Before Reviewing" — > Edit Restrict Field Visibility. You can check which fields are visible to reviewers by going to any submission page and hovering over the eye icon next to the field name.
{% endhint %}

## Common Customizations

### Asking authors to agree to conference policies

<figure><img src="https://622636955-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FVorH499wd7ipUjYX5etp%2Fuploads%2FOpLSJksv0cxqMsABr5RS%2FScreenshot%202024-08-20%20at%2011.13.02%E2%80%AFAM.png?alt=media&#x26;token=6abf77a1-4bf8-4652-aaa7-beb548497773" alt=""><figcaption></figcaption></figure>

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

The `readers` field can be used to list who will be allowed to read a specific field of the submission form. The example below limits the readers of the `private comments` field to just authors, Assigned Senior Area Chairs, and Program Chairs.

Note: Authors will not be able to read these fields if they are not in the readers list

<figure><img src="https://622636955-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FVorH499wd7ipUjYX5etp%2Fuploads%2FvU0GWDltsGL0FoLTw9Qp%2FScreenshot%202024-08-20%20at%2011.17.59%E2%80%AFAM.png?alt=media&#x26;token=0189e96d-6af2-4aa0-bca8-d83203319f4a" alt=""><figcaption></figcaption></figure>

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

Once you have [reviewed our support for "tracks" in a single venue](../../../how-to-guides/workflow/how-to-have-different-tracks-or-types-of-submissions-for-a-single-venue.md) and you think this is what your venue needs, you can add a "track" field to your submission form if you are using separate reviewing pools for track submissions.

<figure><img src="https://622636955-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FVorH499wd7ipUjYX5etp%2Fuploads%2FtMSUgpKAXIlaIM8o8iLx%2FScreenshot%202024-08-20%20at%2011.23.48%E2%80%AFAM.png?alt=media&#x26;token=4a00f07e-9a91-4285-99a7-63318e77b0b6" alt=""><figcaption></figcaption></figure>

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
