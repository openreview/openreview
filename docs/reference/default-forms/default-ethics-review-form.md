# Default Ethics Review Form

```json
{
  "recommendation": {
    "order": 1,
    "value": {
      "param": {
        "type": "string",
        "input": "radio",
        "enum": [
          "1: No serious ethical issues",
          "2: Serious ethical issues that need to be addressed in the final version",
          "3: Paper should be rejected due to ethical issues"
        ]
      }
    }
  },
  "ethics_review": {
    "order": 2,
    "description": "Provide justification for your suggested ethics issues. Add formatting using Markdown and formulas using LaTeX. For more information see https://openreview.net/faq.",
    "value": {
      "param": {
        "type": "string",
        "maxLength": 200000,
        "markdown": true,
        "input": "textarea",
        "optional": true,
        "deletable": true
      }
    }
  }
}
```

#### Preview

<figure><img src="https://622636955-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FVorH499wd7ipUjYX5etp%2Fuploads%2Fgit-blob-ffa1a47f05006893a8573462135b18a5e4baeb12%2Fdefault-ethics-review-form-preview.png?alt=media" alt="The default ethics review form, showing the Recommendation and Ethics Review fields"><figcaption><p>The default ethics review form as an ethics reviewer sees it.</p></figcaption></figure>
