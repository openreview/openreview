# Default Comment Form

#### API V1 JSON

```json
{
  "title": {
      "order": 0,
      "value-regex": ".{1,500}",
      "description": "Brief summary of your comment.",
      "required": true
  },
  "comment": {
      "order": 1,
      "value-regex": "[\\S\\s]{1,5000}",
      "description": "Your comment or reply (max 5000 characters). Add formatting using Markdown and formulas using LaTeX. For more information see https://openreview.net/faq",
      "required": true,
      "markdown": true
  }
}
```

#### API V2 JSON

```json
{
  "title": {
    "value": {
      "param": {
        "type": "string",
        "regex": ".{1,500}"
      }
    },
    "order": 0,
    "description": "Brief summary of your comment."
  },
  "comment": {
    "value": {
      "param": {
        "type": "string",
        "regex": "[\\S\\s]{1,5000}",
        "markdown": true
      }
    },
    "order": 1,
    "description": "Your comment or reply (max 5000 characters). Add formatting using Markdown and formulas using LaTeX. For more information see https://openreview.net/faq"
  }
}
```

#### Preview

![Commen](https://openreview.net/images/faq-comment-form.png)
