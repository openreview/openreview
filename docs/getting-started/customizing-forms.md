---
description: >-
  A guide for program chairs on how to customize the different forms available
  to the reviewing committee.
---

# Customizing Forms

## Where to customize forms

The form fields for all invitations are modified via the [Workflow Timeline](https://docs.openreview.net/conference-review-workflow/overview#id-2.-accessing-the-workflow-console). Here's an example of where to edit the fields for the submission form.

<figure><img src="https://622636955-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FVorH499wd7ipUjYX5etp%2Fuploads%2FCydGySFn9NyOnfoBJg9D%2FScreenshot%202026-06-10%20at%2010.55.55%E2%80%AFAM.png?alt=media&#x26;token=1e218b4a-cb45-4a4f-bcca-9f796d7eda2e" alt="This image is a screenshot of the Submission invitation edit interface. It is cropped to focus on the section on editing form fields." width="375"><figcaption><p>Screenshot of the Submission Invitation edit interface,<br>focused on how to edit form fields.</p></figcaption></figure>

However, at this time, `readers` can only be added to fields via the JSON content tab. Please see [this guide](customizing-forms.md#setting-the-readers-of-a-field) about adding readers to fields to limit which groups can see them.

<figure><img src="https://622636955-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FVorH499wd7ipUjYX5etp%2Fuploads%2FeyqA0NL6BebkGRRzGuWP%2FScreenshot%202026-06-10%20at%2010.56.07%E2%80%AFAM.png?alt=media&#x26;token=c70dfce3-e367-4ea3-9ada-95c940ab36fe" alt="Screenshot of the Edit form fields interface, focused on the various tabs starting with left to right: &#x22;Content JSON&#x22;, &#x22;Widgets&#x22;, and &#x22;Preview&#x22;" width="375"><figcaption><p>Screenshot of the Edit Form Fields interface,<br>and the tabs to edit and preview the fields.</p></figcaption></figure>

## Essential structure of custom fields

These text boxes accept a valid JSON object with fields and values. The following is an example where the _title_ field gets replaced with a radio button, like so:

<figure><img src="https://622636955-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FVorH499wd7ipUjYX5etp%2Fuploads%2FtgMLfYGnay2sk6Y7VdAS%2Fimage.png?alt=media&#x26;token=b38f62ce-f5fb-4df3-ad8d-b4af4627ae62" alt="A required Title field rendered as a radio button with the single option Test Submission Title, under the description Title of the paper."><figcaption><p>Preview of how this field will be rendered on OpenReview</p></figcaption></figure>

```
{
    "title": {
        "order": 1,
        "description": "Title of the paper",
        "value": {
            "param": {
                "type": "string",
                "enum": ["Test Submission Title"],
                "input": "radio"
            }
        }
    }
}
```

Note that correct indentation levels and matched brackets are necessary for valid JSON.

The primary fields of the entry are:

`title` - This will be the name of the field in the form

`order` - Determines where in the form the field will appear

`description` - Will show up in the form as instructions or description

`value` - Will have subfields (under `param`) determining the format of the field and the options for responses

```
"value": {
    "param": {
        "type": "string",
        "enum": ["Test Submission Title"],
        "input": "radio"
    }
}
```

When making a field that is asking for user input, you will _**always**_ see this pattern of `"value": { "param": {...} }`. Inside the `param` object are fields determining what the user sees in the input form along with what the user is allowed to submit: these are _representation specifiers_ and _validation specifiers_.

{% hint style="danger" %}
Both validation and representation specifiers can be found inside the `param` object
{% endhint %}

## Specifiers

This section will introduce common specifiers used in customizing forms. Further information about specifiers can be found [here](../reference/api-v2/entities/invitation/specifiers.md).

### Representation Specifiers

Representation specifiers determine how the user will input their response into the field (for example a textbox or a checklist). These will be defined in the `param` object.

The `input` specifier determines the rendering on the form and can have the following values (see below for examples of how different input types render):

* `text`
* `select`
* `checkbox`
* `textarea`
* `radio`

`default` is the default value of the box that will appear when the widget is initialized

`markdown` is a boolean value (true/false) that enables markdown for this text field

`scroll` is a boolean that adds a scroll bar to a `textarea` input

### Validation Specifiers

Validation specifiers are used by the back-end to ensure data submitted through the form conforms to certain requirements. In the example above, only a single string is allowed, and that string must be one of the values defined in the `enum` array. Specifically, a string that has the value "`Test Submission Title`"

`optional` is a boolean (true/false) value that indicates whether or not this field is required to be present when the form is submitted. By default all fields in the form are required, and you can add `optional : true` to indicate an optional field.

{% hint style="info" %}
Required fields have their field names prefixed with an asterisk in the preview tab and note edit interface.
{% endhint %}

`type` specifiers require the input to be of a specific type: options are `string`, `string[]` (string array) and `file`.

`string` fields can be further validated by using fields to describe the structure of a valid string input. Some of these field are:

* `"maxLength":` set the maximum number of characters of the input
* `"minLength":` set the minimum number of characters of the input
* `"regex":` use regular expressions to define acceptable string structures
* `"enum":` restrict the user to a predefined set of strings
* `"items":` an array of strings as indicated by its type (only used with `type: string[]` ) \*\*

\*\* All values in `"items"` will be considered required unless specified otherwise with `"optional": true`. Please see [multiple choices](customizing-forms.md#multiple-choices) for an example.

`file` fields are specifically validated with `maxSize` and `extensions`. `maxSize` is an integer that specifies the size of the largest file that can be uploaded on the form in megabytes. `extensions` is a list of strings that are extensions, for example `"extensions": ["pdf", "zip"]`.

{% hint style="warning" %}
Extensions that have a "." in them are **not** supported. The following field would be invalid because "`tar.gz`" is not supported:

```
"extensions": ["zip", "tar.gz"]
```
{% endhint %}

## Examples of Common Form Patterns

### Text Input

{% tabs %}
{% tab title="Small Text Area" %}
```
{
  "title": {
    "order": 1,
    "description": "(Optional) Brief summary of your comment.",
    "value": {
      "param": {
        "type": "string",
        "maxLength": 500,
        "optional": true
      }
    }
  }
}
```

<figure><img src="https://622636955-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FVorH499wd7ipUjYX5etp%2Fuploads%2F5Ydl6krLLqyIJtFb81cO%2FcommentTitle.png?alt=media&#x26;token=45ce94f3-f00a-4ef6-bdbe-cfa32169a702" alt=""><figcaption></figcaption></figure>
{% endtab %}

{% tab title="Large Text Area" %}
```
{
  "comment": {
    "order": 2,
    "description": "Your comment or reply (max 5000 characters). Add formatting using Markdown and formulas using LaTeX. For more information see https://openreview.net/faq",
    "value": {
      "param": {
        "type": "string",
        "maxLength": 5000,
        "markdown": true,
        "input": "textarea"
      }
    }
  }
}
```

<figure><img src="https://622636955-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FVorH499wd7ipUjYX5etp%2Fuploads%2FDJmRQ7jDtAUHkPVL5wXz%2Fcommentcomment.png?alt=media&#x26;token=13688e64-2ce8-49d6-9654-744fd191bc6f" alt=""><figcaption></figcaption></figure>
{% endtab %}
{% endtabs %}

### Single Choice

{% tabs %}
{% tab title="Radio Button" %}
```
{
  "recommendation": {
    "order": 10,
    "description": "Please provide your recommendation based on the manuscript, reviews, author responses and your discussion with the reviewers.",
    "value": {
      "param": {
        "type": "string",
        "enum": [
          "Reject",
          "Accept",
          "Nominate for best paper"
        ],
        "input": "radio"
      }
    }
  }
}
```

<figure><img src="https://622636955-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FVorH499wd7ipUjYX5etp%2Fuploads%2F72wbZpO6KSjaKfFlt1jO%2Fmetareviewupdate.png?alt=media&#x26;token=4711afc8-e716-44f8-b6ac-703a73dc7eae" alt=""><figcaption></figcaption></figure>
{% endtab %}

{% tab title="Dropdown" %}
```
{
  "submission_track": {
    "order": 11,
    "description": "Please select a preferred track, which will be considered in the paper-reviewer assignment. Please note that your choice of a track will be taken into account, but the chairs may decide to move your paper to a different area when appropriate.",
    "value": {
      "param": {
        "type": "string",
        "enum": [
          "Commonsense Reasoning",
          "Computational Social Science and Cultural Analytics",
          "Dialogue and Interactive Systems",
          "Discourse and Pragmatics",
          "Efficient Methods for NLP",
          "Ethics in NLP",
          "Human-Centered NLP",
          "Information Extraction",
          "Information Retrieval and Text Mining",
          "Interpretability, Interactivity, and Analysis of Models for NLP",
          "Language Grounding to Vision, Robotics and Beyond",
          "Language Modeling and Analysis of Language Models",
          "Linguistic Theories, Cognitive Modeling, and Psycholinguistics",
          "Machine Learning for NLP",
          "Machine Translation",
          "Multilinguality and Linguistic Diversity",
          "Natural Language Generation",
          "NLP Applications",
          "Phonology, Morphology, and Word Segmentation",
          "Question Answering",
          "Resources and Evaluation",
          "Semantics: Lexical",
          "Semantics: Lexical, Sentence level, Document Level, Textual Inference, etc.",
          "Sentiment Analysis, Stylistic Analysis, and Argument Mining",
          "Speech and Multimodality",
          "Summarization",
          "Syntax, Parsing and their Applications",
          "Theme Track: Large Language Models and the Future of NLP"
        ],
        "input": "select"
      }
    }
  }
}
```

<figure><img src="https://622636955-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FVorH499wd7ipUjYX5etp%2Fuploads%2FVjwSyouj3iCsiyFZT0nI%2Fdropdownselect.png?alt=media&#x26;token=8352cf5f-be1a-447f-8846-e4eb0aa24606" alt=""><figcaption></figcaption></figure>
{% endtab %}
{% endtabs %}

### Multiple Choices

{% tabs %}
{% tab title="Checkboxes" %}
```
{
  "confirmation_of_submission_requirements": {
    "order": 17,
    "description": "Before you submit this paper, please make sure that the following requirements are met. If any of these requirements are not fulfilled, your submission will be rejected and will not be reviewed.",
    "value": {
      "param": {
        "type": "string[]",
        "optional": true,
        "items": [
          { "value": "Requirement 1", "description": "Requirement 1", "optional": true},
          { "value": "Requirement 2", "description": "Requirement 1", "optional": true}
        ],
        "input": "checkbox"
      }
    }
  }
}
```

<figure><img src="https://622636955-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FVorH499wd7ipUjYX5etp%2Fuploads%2FEbznFBDevTgTF7RTnIB3%2Frequirements.png?alt=media&#x26;token=c3b91a90-baf6-4824-8789-e1cd8a61a68d" alt=""><figcaption></figcaption></figure>
{% endtab %}

{% tab title="Dropdown Values" %}
<pre><code>{
  "submission_tracks": {
    "order": 11,
    "description": "Please select your preferred tracks, which will be considered in the paper-reviewer assignment. Please note that your choice of tracks will be taken into account, but the chairs may decide to move your paper to a different area when appropriate.",
    "value": {
      "param": {
        "type": "string[]",
        "items": [
            { "value": "Commonsense Reasoning", "description": "Commonsense Reasoning", "optional": true },
            { "value": "Computational Social Science and Cultural Analytics", "description": "Computational Social Science and Cultural Analytics", "optional": true },
            { "value": "Dialogue and Interactive Systems", "description": "Dialogue and Interactive Systems", "optional": true },
            { "value": "Discourse and Pragmatics", "description": "Discourse and Pragmatics", "optional": true },
            { "value": "Efficient Methods for NLP", "description": "Efficient Methods for NLP", "optional": true },
            { "value": "Ethics in NLP", "description": "Ethics in NLP", "optional": true },
            { "value": "Human-Centered NLP", "description": "Human-Centered NLP", "optional": true },
<strong>            { "value": "Information Extraction", "description": "Information Extraction", "optional": true },
</strong>            { "value": "Information Retrieval and Text Mining", "description": "Information Retrieval and Text Mining", "optional": true },
            { "value": "Interpretability, Interactivity, and Analysis of Models for NLP", "description": "Interpretability, Interactivity, and Analysis of Models for NLP", "optional": true },
            { "value": "Language Grounding to Vision, Robotics and Beyond", "description": "Language Grounding to Vision, Robotics and Beyond", "optional": true },
            { "value": "Language Modeling and Analysis of Language Models", "description": "Language Modeling and Analysis of Language Models", "optional": true },
            { "value": "Linguistic Theories, Cognitive Modeling, and Psycholinguistics", "description": "Linguistic Theories, Cognitive Modeling, and Psycholinguistics", "optional": true },
            { "value": "Machine Learning for NLP", "description": "Machine Learning for NLP", "optional": true },
            { "value": "Machine Translation", "description": "Machine Translation", "optional": true },
            { "value": "Multilinguality and Linguistic Diversity", "description": "Multilinguality and Linguistic Diversity", "optional": true },
            { "value": "Natural Language Generation", "description": "Natural Language Generation", "optional": true },
<strong>            { "value": "NLP Applications", "description": "NLP Applications", "optional": true },
</strong><strong>            { "value": "Phonology, Morphology, and Word Segmentation", "description": "Phonology, Morphology, and Word Segmentation", "optional": true },
</strong>            { "value": "Question Answering", "description": "Question Answering", "optional": true },
            { "value": "Resources and Evaluation", "description": "Resources and Evaluation", "optional": true },
            { "value": "Semantics: Lexical", "description": "Semantics: Lexical", "optional": true },
<strong>            { "value": "Semantics: Lexical, Sentence level, Document Level, Textual Inference, etc.", "description": "Semantics: Lexical, Sentence level, Document Level, Textual Inference, etc.", "optional": true },
</strong>            { "value": "Sentiment Analysis, Stylistic Analysis, and Argument Mining", "description": "Sentiment Analysis, Stylistic Analysis, and Argument Mining", "optional": true },
            { "value": "Speech and Multimodality", "description": "Speech and Multimodality", "optional": true },
            { "value": "Summarization", "description": "Summarization", "optional": true },
            { "value": "Syntax, Parsing and their Applications", "description": "Syntax, Parsing and their Applications", "optional": true },
            { "value": "Theme Track: Large Language Models and the Future of NLP", "description": "Theme Track: Large Language Models and the Future of NLP", "optional": true }
        ],
        "input": "select"
      }
    }
  }
}
</code></pre>

<figure><img src="https://622636955-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FVorH499wd7ipUjYX5etp%2Fuploads%2Fdy0pDDQb79UJ1KAlLLtG%2Fdropdownvalues.png?alt=media&#x26;token=e835b7cd-3a3d-45db-8b58-76b64d829c6d" alt=""><figcaption></figcaption></figure>
{% endtab %}
{% endtabs %}

### Miscellaneous

{% tabs %}
{% tab title="Files" %}
```
{
  "supplementary_material": {
    "order": 10,
    "description": "All supplementary material must be self-contained and zipped into a single file. Note that supplementary material will be visible to reviewers and the public throughout and after the review period, and ensure all material is anonymized. The maximum file size is 200MB.",
    "value": {
      "param": {
        "type": "file",
        "extensions": [
          "zip",
          "pdf",
          "tgz",
          "gz"
        ],
        "maxSize": 200,
        "optional": true
      }
    }
  }
}
```

<figure><img src="https://622636955-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FVorH499wd7ipUjYX5etp%2Fuploads%2FB2STzNHT0nZLvE1pGKYC%2Fsubmissionsupplementary.png?alt=media&#x26;token=3118eb6a-9848-43ad-a90b-ac62a49b17ca" alt=""><figcaption></figcaption></figure>
{% endtab %}
{% endtabs %}

### Setting the Readers of a Field

If you want to limit who in the committee can see a particular field in a form, this is done by adding a `readers` field. Please follow this link for more detailed information on [hiding or revealing fields](../how-to-guides/submissions-comments-reviews-and-decisions/how-to-hide-reveal-fields.md). Below are two examples, one for the [submission form](hosting-a-venue-on-openreview/customizing-your-submission-form/) and one for the meta review form. Notice the different use of dollar sign notation. The notation used for the meta review form will also work for other replies to the forum: reviews, comments, and decisions. Please scroll to see how best to setup `readers` depending on which venue configuration was deployed.

{% tabs %}
{% tab title="Submission" %}
```
{
  "supplementary_material": {
    "order": 10,
    "description": "All supplementary material must be self-contained and zipped into a single file. Note that supplementary material will be visible to reviewers and the public throughout and after the review period, and ensure all material is anonymized. The maximum file size is 200MB.",
    "value": {
      "param": {
        "type": "file",
        "extensions": [
          "zip",
          "pdf",
          "tgz",
          "gz"
        ],
        "maxSize": 200,
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
{% endtab %}

{% tab title="Meta Review" %}
```
{
  "recommendation": {
    "order": 10,
    "description": "Please provide your recommendation based on the manuscript, reviews, author responses and your discussion with the reviewers.",
    "value": {
      "param": {
        "type": "string",
        "enum": [
          "Accept (Oral)",
          "Accept (Poster)",
          "Reject"
        ],
        "input": "radio"
      }
    },
    "readers": [
    "Your/Venue/ID/Program_Chairs",
    "Your/Venue/ID/Submission${7/content/noteNumber/value}/Area_Chairs",
    "Your/Venue/ID/Submission${7/content/noteNumber/value}/Reviewers"
  ]
  }
}
```
{% endtab %}
{% endtabs %}
