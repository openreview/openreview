# Default Submission Form

```json
{
  "title": {
    "order": 1,
    "description": "Title of paper. Add TeX formulas using the following formats: $In-line Formula$ or $$Block Formula$$.",
    "value": {
      "param": {
        "type": "string",
        "regex": "^.{1,250}$"
      }
    }
  },
  "authors": {
    "order": 2,
    "description": "Search author profile by name or profile ID. All authors must have an OpenReview profile prior to submitting a paper.",
    "value": {
      "param": {
        "type": "author{}",
        "properties": {
          "fullname": {
            "param": {
              "type": "string"
            }
          },
          "username": {
            "param": {
              "type": "string",
              "regex": "^~\\S+$",
              "mismatchError": "must be a valid profile ID"
            }
          },
          "institutions": {
            "param": {
              "type": "object{}",
              "properties": {
                "name": {
                  "param": {
                    "type": "string"
                  }
                },
                "domain": {
                  "param": {
                    "type": "string"
                  }
                },
                "country": {
                  "param": {
                    "type": "string"
                  }
                }
              },
              "optional": true
            }
          }
        }
      }
    }
  },
  "keywords": {
    "description": "Comma separated list of keywords.",
    "order": 4,
    "value": {
      "param": {
        "type": "string[]",
        "regex": ".+"
      }
    }
  },
  "TLDR": {
    "order": 5,
    "description": "\"Too Long; Didn't Read\": a short sentence describing your paper",
    "value": {
      "param": {
        "fieldName": "TL;DR",
        "type": "string",
        "maxLength": 250,
        "optional": true,
        "deletable": true
      }
    }
  },
  "abstract": {
    "order": 6,
    "description": "Abstract of paper. Add TeX formulas using the following formats: $In-line Formula$ or $$Block Formula$$.",
    "value": {
      "param": {
        "type": "string",
        "maxLength": 5000,
        "markdown": true,
        "input": "textarea"
      }
    }
  },
  "pdf": {
    "order": 7,
    "description": "Upload a PDF file that ends with .pdf.",
    "value": {
      "param": {
        "type": "file",
        "maxSize": 50,
        "extensions": [
          "pdf"
        ]
      }
    }
  }
}
```

#### Preview

<figure><img src="https://622636955-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FVorH499wd7ipUjYX5etp%2Fuploads%2Fgit-blob-7674cfc829e37c61c3e92c5e9679b4ba5173f837%2Fdefault-submission-form-preview.png?alt=media" alt="The default submission form, showing the Title, Authors, Keywords, TL;DR, Abstract and PDF fields"><figcaption><p>The default submission form as an author sees it.</p></figcaption></figure>
