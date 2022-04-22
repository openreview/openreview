# Default Comment Form

#### JSON

```
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

#### Preview

![Commen](https://openreview.net/images/faq-comment-form.png)
