# Default Review Form

```json
{
  "title": {
    "order": 1,
    "description": "Brief summary of your review.",
    "value": {
      "param": {
        "type": "string",
        "regex": ".{0,500}"
      }
    }
  },
  "review": {
    "order": 2,
    "description": "Please provide an evaluation of the quality, clarity, originality and significance of this work, including a list of its pros and cons (max 200000 characters). Add formatting using Markdown and formulas using LaTeX. For more information see https://openreview.net/faq",
    "value": {
      "param": {
        "type": "string",
        "maxLength": 200000,
        "markdown": true,
        "input": "textarea"
      }
    }
  },
  "rating": {
    "order": 3,
    "value": {
      "param": {
        "type": "integer",
        "enum": [
          {
            "value": 10,
            "description": "10: Top 5% of accepted papers, seminal paper"
          },
          {
            "value": 9,
            "description": "9: Top 15% of accepted papers, strong accept"
          },
          {
            "value": 8,
            "description": "8: Top 50% of accepted papers, clear accept"
          },
          {
            "value": 7,
            "description": "7: Good paper, accept"
          },
          {
            "value": 6,
            "description": "6: Marginally above acceptance threshold"
          },
          {
            "value": 5,
            "description": "5: Marginally below acceptance threshold"
          },
          {
            "value": 4,
            "description": "4: Ok but not good enough - rejection"
          },
          {
            "value": 3,
            "description": "3: Clear rejection"
          },
          {
            "value": 2,
            "description": "2: Strong rejection"
          },
          {
            "value": 1,
            "description": "1: Trivial or wrong"
          }
        ],
        "input": "radio"
      }
    }
  },
  "confidence": {
    "order": 4,
    "value": {
      "param": {
        "type": "integer",
        "enum": [
          {
            "value": 5,
            "description": "5: The reviewer is absolutely certain that the evaluation is correct and very familiar with the relevant literature"
          },
          {
            "value": 4,
            "description": "4: The reviewer is confident but not absolutely certain that the evaluation is correct"
          },
          {
            "value": 3,
            "description": "3: The reviewer is fairly confident that the evaluation is correct"
          },
          {
            "value": 2,
            "description": "2: The reviewer is willing to defend the evaluation, but it is quite likely that the reviewer did not understand central parts of the paper"
          },
          {
            "value": 1,
            "description": "1: The reviewer's evaluation is an educated guess"
          }
        ],
        "input": "radio"
      }
    }
  }
}
```

#### Preview

<figure><img src="https://622636955-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FVorH499wd7ipUjYX5etp%2Fuploads%2Fgit-blob-eeba98001f3cd76380b16f6ec93c768673cbf753%2Fdefault-review-form-preview.png?alt=media" alt="The default review form, showing the Title, Review, Rating and Confidence fields"><figcaption><p>The default review form as a reviewer sees it.</p></figcaption></figure>
