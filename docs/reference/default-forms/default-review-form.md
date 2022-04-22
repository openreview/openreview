# Default Review Form

#### JSON

```
 {
  "title": {
      "order": 1,
      "value-regex": ".{0,500}",
      "description": "Brief summary of your review.",
      "required": true
  },
  "review": {
      "order": 2,
      "value-regex": "[\\S\\s]{1,200000}",
      "description": "Please provide an evaluation of the quality, clarity, originality and significance of this work, including a list of its pros and cons (max 200000 characters). Add formatting using Markdown and formulas using LaTeX. For more information see https://openreview.net/faq",
      "required": true,
      "markdown": true
  },
  "rating": {
      "order": 3,
      "value-dropdown": [
          "10: Top 5% of accepted papers, seminal paper",
          "9: Top 15% of accepted papers, strong accept",
          "8: Top 50% of accepted papers, clear accept",
          "7: Good paper, accept",
          "6: Marginally above acceptance threshold",
          "5: Marginally below acceptance threshold",
          "4: Ok but not good enough - rejection",
          "3: Clear rejection",
          "2: Strong rejection",
          "1: Trivial or wrong"
      ],
      "required": true
  },
  "confidence": {
      "order": 4,
      "value-radio": [
          "5: The reviewer is absolutely certain that the evaluation is correct and very familiar with the relevant literature",
          "4: The reviewer is confident but not absolutely certain that the evaluation is correct",
          "3: The reviewer is fairly confident that the evaluation is correct",
          "2: The reviewer is willing to defend the evaluation, but it is quite likely that the reviewer did not understand central parts of the paper",
          "1: The reviewer's evaluation is an educated guess"
      ],
      "required": true
  }
}
```

#### Preview

![Review](https://openreview.net/images/faq-review-form.png)
