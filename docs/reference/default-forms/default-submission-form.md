# Default Submission Form

#### JSON

```
{
  "title": {
    "description": "Title of paper. Add TeX formulas using the following formats: $In-line Formula$ or $$Block Formula$$",
    "order": 1,
    "value-regex": ".{1,250}",
    "required": true
  },
  "authors": {
      "description": "Comma separated list of author names.",
      "order": 2,
      "values-regex": "[^;,\\n]+(,[^,\\n]+)*",
      "required": true,
      "hidden" true
  },
  "authorids": {
      "description": "Search author profile by first, middle and last name or email address. If the profile is not found, you can add the author by completing first, middle, and last names as well as author email address.",
      "order": 3,
      "values-regex": "~.*|([a-z0-9_\\-\\.]{1,}@[a-z0-9_\\-\\.]{2,}\\.[a-z]{2,},){0,}([a-z0-9_\\-\\.]{1,}@[a-z0-9_\\-\\.]{2,}\\.[a-z]{2,})",
      "required": true
  },
  "keywords": {
      "description": "Comma separated list of keywords.",
      "order": 6,
      "values-regex": "(^$)|[^;,\\n]+(,[^,\\n]+)*"
  },
  "TL;DR": {
      "description": "\"Too Long; Didn't Read\": a short sentence describing your paper",
      "order": 7,
      "value-regex": "[^\\n]{0,250}",
      "required": false
  },
  "abstract": {
      "description": "Abstract of paper. Add TeX formulas using the following formats: $In-line Formula$ or $$Block Formula$$",
      "order": 8,
      "value-regex": "[\\S\\s]{1,5000}",
      "required": true
  },
  "pdf": {
      "description": "Upload a PDF file that ends with .pdf",
      "order": 9,
      "value-file": {
          "fileTypes": [
              "pdf"
          ],
          "size": 50
      },
      "required": true
  }
}
```

#### Preview&#x20;

![](<../../.gitbook/assets/image (10).png>)
