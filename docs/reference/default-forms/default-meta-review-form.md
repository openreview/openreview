# Default Meta Review Form

#### JSON

```
{
  "metareview": {
      "order": 1,
      "value-regex": "[\\S\\s]{1,5000}",
      "description": "Please provide an evaluation of the quality, clarity, originality and significance of this work, including a list of its pros and cons. Your comment or reply (max 5000 characters). Add formatting using Markdown and formulas using LaTeX. For more information see https://openreview.net/faq",
      "required": true,
      "markdown": true
  },
  "recommendation": {
      "order": 2,
      "value-dropdown": [
          "Accept (Oral)",
          "Accept (Poster)",
          "Reject"
      ],
      "required": true
  },
  "confidence": {
      "order": 3,
      "value-radio": [
          "5: The area chair is absolutely certain",
          "4: The area chair is confident but not absolutely certain",
          "3: The area chair is somewhat confident",
          "2: The area chair is not sure",
          "1: The area chair's evaluation is an educated guess"
      ],
      "required": true
  }
}
```

#### Preview

![](<../../.gitbook/assets/image (8).png>)
