# Default Rebuttal Form

**API V2 JSON**

```json
{
    "rebuttal": {
        "order": 1,
        "description": "Rebuttals can include Markdown formatting and LaTeX forumulas, for more information see https://openreview.net/faq , max length: 2500",
        "value": {
            "param": {
                "type": "string",
                "maxLength": 2500,
                "markdown": true,
                "input": "textarea"
            }
        }
    }
}
```

**Preview**

<figure><img src="../../.gitbook/assets/Screen Shot 2023-11-20 at 2.27.09 PM.png" alt=""><figcaption></figcaption></figure>
