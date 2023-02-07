# Invitation

Users can enter data into the system using an invitation. An invitation is roughly a template that indicates required and valid values that will be saved to the database. The invitation contains a list of fields that need to be completed by the user.

When modifying an Invitation's reply field, each field must be a valid JSON with a title and the following optional properties (with the exception of field type, which is required):

* field type: the type of the field, which includes _value(s)_, _value(s)-regex_, _value-radio_, _value(s)-checkbox_, _value(s)-dropdown_, _value-file_
* description: a string describing how users should fill this field
* order: a number representing the position in which the field will appear on the form
* required: a boolean representing whether the field is required (true) or optional (false)
* default: the default value of the field
* markdown: a boolean representing whether Markdown is enabled for the field (only valid for _value-regex_ field type)

You can have different types of fields:

*   **value**, **values**: string or array of strings; the value(s) cannot be modified by the user.

    ```
    "title": {
      "value": "this is a static value"
    },
    "keywords": {
      "values": ["Deep Learning", "Machine Learning"]
    }
    ```
*   **value-regex**, **values-regex**: string or array of strings; the value entered by the user should pass the regex validation.

    ```
    "title": {
      "order": 0,
      "value-regex": ".{1,500}",
      "description": "Brief summary of your comment.",
      "required": true,
      "markdown": true
    },
    "emails": {
      "description": "Comma separated list of author email addresses, lowercased, in the same order as above. For authors with existing OpenReview accounts, please make sure that the provided email address(es) match those listed in the author's profile.",
      "order": 3,
      "values-regex": "([a-z0-9_\\-\\.]{1,}@[a-z0-9_\\-\\.]{2,}\\.[a-z]{2,},){0,}([a-z0-9_\\-\\.]{1,}@[a-z0-9_\\-\\.]{2,}\\.[a-z]{2,})",
      "required": true
    }
    ```
*   **value-radio**: string or array of strings; the user can only choose one option.

    ```
    "confirmation": {
      "description": "Please confirm you have read the workshop's policies.",
      "value-radio": [
          "I have read and agree with the workshop's policy on behalf of myself and my co-authors."
      ],
      "order": 2,
      "required": true
    },
    "soundness": {
      "description": "Indicate your agreement with the following: This paper is technically sound.",
      "value-radio": [
          "Agree",
          "Neutral",
          "Disagree"
      ],
      "order": 3,
      "required": true
    }
    ```
*   **value-checkbox**, **values-checkbox**: string or array of strings; the user can select one or more options.

    ```
    "profile_confirmed": {
      "description": "Please confirm that your OpenReview profile is up-to-date by selecting 'Yes'.",
      "value-checkbox": "Yes",
      "required": true,
      "order": 1
    },
    "keywords": {
      "description": "Please check all keywords that apply.",
        "values-checkbox": [
          "Deep Learning",
          "Machine Learning",
          "Computer Vision",
          "Database Design"
        ],
        "required": true,
        "order": 3
    }
    ```
*   **value-dropdown**, **values-dropdown**: array of strings; the user can select one or more options from a dropdown.

    ```
    "novelty": {
      "order": 2,
      "value-dropdown": ["Very High", "High", "Neutral", "Low", "Very Low"],
      "description": "Indicate your agreement with the following: This paper is highly novel.",
      "required": true
    },
    "keywords": {
      "order" : 5,
      "description" : "Select or type subject area",
      "values-dropdown": [
        "Computer Vision Theory",
        "Dataset and Evaluation",
        "Human Computer Interaction",
        "Machine Learning",
        "Unsupervised Learning"
      ],
      "required": true
    }
    ```
*   **value-file**: a valid JSON specifying the expected upload file type and size in MB; the user can upload one file.

    The _fileTypes_ field expects an array of strings specifying the permitted file types that the user can upload. Supported field types are pdf, csv, zip and mp4.

    ```
    "pdf": {
      "description": "Upload a single PDF file or a single mp4 file.",
      "order": 6,
      "value-file": {
          "fileTypes": ["pdf", "mp4"],
          "size": 50
      },
      "required":true
    }
    ```
