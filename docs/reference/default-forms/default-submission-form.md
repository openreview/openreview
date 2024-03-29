# Default Submission Form

```json
{
  "title": {
    "value": {
      "param": {
        "type": "string",
        "regex": ".{1,250}"
      }
    },
    "description": "Title of paper. Add TeX formulas using the following formats: $In-line Formula$ or $$Block Formula$$",
    "order": 1
  },
  "authors": {
    "value": {
      "param": {
        "type": "string[]",
        "regex": "[^;,\\n]+(,[^,\\n]+)*",
        "hidden": true
      }
    },
    "description": "Comma separated list of author names.",
    "order": 2
  },
  "authorids": {
    "value": {
      "param": {
        "type": "group[]",
        "regex": "~.*|([a-z0-9_\\-\\.]{1,}@[a-z0-9_\\-\\.]{2,}\\.[a-z]{2,},){0,}([a-z0-9_\\-\\.]{1,}@[a-z0-9_\\-\\.]{2,}\\.[a-z]{2,})"
      }
    },
    "description": "Search author profile by first, middle and last name or email address. If the profile is not found, you can add the author by completing first, middle, and last names as well as author email address.",
    "order": 3
  },
  "keywords": {
    "value": {
      "param": {
        "type": "string",
        "regex": "(^$)|[^;,\\n]+(,[^,\\n]+)*",
        "optional": true,
        "deletable": true
      }
    },
    "description": "Comma separated list of keywords.",
    "order": 6
  },
  "TLDR": {
    "value": {
      "param": {
        "fieldName": "TL;DR",
        "type": "string",
        "maxLength": 250,
        "optional": true,
        "deletable": true
      }
    },
    "description": "\"Too Long; Didn't Read\": a short sentence describing your paper",
    "order": 7
  },
  "abstract": {
    "value": {
      "param": {
        "type": "string",
        "minLength": 1,
        "maxLength": 5000,
        "input": "textarea",
        "markdown": true
      }
    },
    "description": "Abstract of paper. Add TeX formulas using the following formats: $In-line Formula$ or $$Block Formula$$",
    "order": 8
  },
  "pdf": {
    "value": {
      "param": {
        "type": "file",
        "extensions": [ "pdf" ],
        "maxSize": 50
      }
    },
    "description": "Upload a PDF file that ends with .pdf",
    "order": 9
  }
}
```

#### Preview&#x20;

![](<../../.gitbook/assets/image (10).png>)
