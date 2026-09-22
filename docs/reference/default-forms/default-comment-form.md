# Default Comment Form

```json
{
  "title": {
    "order": 1,
    "description": "(Optional) Brief summary of your comment.",
    "value": {
      "param": {
        "type": "string",
        "maxLength": 500,
        "optional": true,
        "deletable": true
      }
    }
  },
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

#### Preview

<figure><img src="https://622636955-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FVorH499wd7ipUjYX5etp%2Fuploads%2Fgit-blob-771daa5c7b4c59237cde37a8a217eb5a7ab9ab6c%2Fdefault-comment-form-preview.png?alt=media" alt="The default comment form, showing the Title, Comment and Readers fields"><figcaption><p>The default comment form, with reader selection enabled.</p></figcaption></figure>
