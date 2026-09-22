# Default Decision Form

```json
{
  "title": {
    "order": 1,
    "value": "Paper Decision"
  },
  "decision": {
    "order": 2,
    "description": "Decision",
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
    }
  },
  "comment": {
    "order": 3,
    "value": {
      "param": {
        "type": "string",
        "markdown": true,
        "input": "textarea",
        "optional": true,
        "deletable": true
      }
    }
  }
}
```

**Preview**

<figure><img src="https://622636955-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FVorH499wd7ipUjYX5etp%2Fuploads%2Fgit-blob-a4b3877c463cd89a31d38da31509bbd9ded3dbfa%2Fdefault-decision-form-preview.png?alt=media" alt="The default decision form, showing the Title, Decision and Comment fields"><figcaption><p>The default decision form as a program chair sees it.</p></figcaption></figure>
