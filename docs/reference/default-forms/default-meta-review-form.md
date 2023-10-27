# Default Meta Review Form

#### API V2 JSON

```json
{
  "metareview": {
    "value": {
      "param": {
        "type": "string",
        "minLength": 1,
        "maxLength": 5000,
        "input": "textarea",
        "markdown": true
      }
    },
    "order": 1,
    "description": "Please provide an evaluation of the quality, clarity, originality and significance of this work, including a list of its pros and cons. Your comment or reply (max 5000 characters). Add formatting using Markdown and formulas using LaTeX. For more information see https://openreview.net/faq"
  },
  "recommendation": {
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
    "order": 2
  },
  "confidence": {
    "value": {
      "param": {
        "type": "string",
        "enum": [
          "5: The area chair is absolutely certain",
          "4: The area chair is confident but not absolutely certain",
          "3: The area chair is somewhat confident",
          "2: The area chair is not sure",
          "1: The area chair's evaluation is an educated guess"
        ],
        "input": "radio"
      }
    },
    "order": 3
  }
}
```

#### API V1 JSON

```json
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

![](<../../.gitbook/assets/image (8) (2).png>)
