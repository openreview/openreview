# Default Decision Form

#### JSON

```
{
  "title": {
      "order": 1,
      "required": true,
      "value": "Paper Decision"
  },
  "decision": {
      "order": 2,
      "required": true,
      "value-radio": [
          "Accept (Oral)",
          "Accept (Poster)",
          "Reject"
      ],
      "description": "Decision"
  },
  "comment": {
      "order": 3,
      "required": false,
      "value-regex": "[\\S\\s]{0,5000}",
      "description": ""
  }
}
```

**Preview**

![](../../.gitbook/assets/image.png)
