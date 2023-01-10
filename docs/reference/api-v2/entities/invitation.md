# Invitation

Users can enter data into the system using an invitation. An invitation is roughly a template that indicates required and valid values that will be saved to the database. The invitation contains a list of fields that need to be completed by the user. These fields that may require some input from the user are called "params" and contain the param field with different properties that indicate valid values for the field.&#x20;

For example:

```json
"title": {
    "value": {
        "param": {
            "type": "string"
        }
    }
}
```

## Specifiers

There are two types of specifiers: Validation Specifiers and Representation Specifiers.

Both specifiers are defined inside the param field. The Representation Specifiers define how the requested data will be rendered in the UI. The Validation Specifiers are used to check the validity of the data.

Validation Specifiers define valid values that can be assigned to the fields of the objects (Edits, Edges, Tags) that will be created using the Invitation. Most Validation Specifiers outside content do not require a type since its type can be inferred from the field name. For instance, the readers field will always be of type group\[]. However, other Validation Specifiers need to be defined for the readers field, such as regex.

{% hint style="info" %}
Only the mandatory fields head and tail in an Edge must specify the field type even though they are not inside a content field.
{% endhint %}

Validation Specifiers inside content will always require a type. This is because content can have any field name and its valid values need to be specified so that the UI can render them and the backend validate them.

In most cases, you can only have one Validation Specifier. The only exceptions are value ranges specifiers, such as minimum, maximum, minLength and maxLength.

There is no restriction on the amount of Representation Specifiers.

### Value Pattern or Validation Specifier

The param should have one of the following specifiers needed to validate the expected value.&#x20;

* const: restricts the value of the field to whatever is written in const. When const is present, the user does not need to pass the value, since it will be assigned by the invitation. In the example below, the title value has to be exactly "This is a title".

```json
"title": {
    "value": {
        "param": {
            "type": "string"
            "const": "This is a title"
        }
    }
}
```

{% hint style="info" %}
The constant above can also be represented with the following:

```
"title": { "value": "This is a title" }
```
{% endhint %}

* enum: valid values must be listed in the enum array. The values in enum must be of the same type and should match the type specified in the field type. When the type of enum is string its values can be regexes too. In the example below "This is a title" and "This issss b regex" are valid values for title.

```json
"title": {
    "value": {
        "param": {
            "type": "string"
            "enum": ["This is a title", "This is{3,5} [a|b] regex"]
        }
    }
}
```

* regex: valid values need to match the specified regex. The type of the field should be string. In the example below, "This asdf title" is a valid value.

```json
"title": {
    "value": {
        "param": {
            "type": "string"
            "regex": "^This .+ title$"
        }
    }
}
```

* range: valid values are within the range \[a, b] which means a <= value <= b. The type of the field must be float or integer. In the example below, the values 0, 3, and 10 are valid.

```json
"grade": {
    "value": {
        "param": {
            "type": "integer"
            "range": [0, 10]
        }
    }
}
```

* withInvitation: valid values are IDs of entities that have as invitation the value specified in withInvitation.

Suppose we have the following note (this is a simplified version of the note):

```json
{
    "id": "or2022",
    "invitations": ["OpenReview.net/2022/Conference/-/Submission"],
    "readers": ["everyone"],
    "writers": ["~Some_Author1"],
    "signatures": ["~Some_Author1"],
    "content": {
        "title": {
            "value": "Title"
        }
    }
}
```

Given the example below, the value "or2022" would be valid.

```json
"id": {
    "param": {
        "withInvitation": "OpenReview.net/2022/Conference/-/Submission"
    }
}
```

* withVenueid: valid values are IDs of entities that have as venueid the value specified in withVenueid.

Suppose we have the following note (this is a simplified version of the note):

```json
{
    "id": "or2023",
    "invitations": ["OpenReview.net/2023/Conference/-/Submission"],
    "readers": ["everyone"],
    "writers": ["~Some_Author1"],
    "signatures": ["~Some_Author1"],
    "content": {
        "title": {
            "value": "Title"
        },
        "venueid": {
            "value": "OpenReview.net/2023/Conference"
        }
    }
}
```

Given the example below, the value "or2023" would be valid.

```json
"id": {
    "param": {
        "withVenueid": "OpenReview.net/2023/Conference"
    }
}
```

* withForum: valid values are IDs of entities that have as forum the value specified in withForum.

Suppose we have the following note (this is a simplified version of the note):

```json
{
    "id": "com2024",
    "invitations": ["OpenReview.net/2024/Conference/-/Submission"],
    "readers": ["everyone"],
    "writers": ["~Some_Author1"],
    "signatures": ["~Some_Author1"],
    "forum": "or2024",
    "content": {
        "title": {
            "value": "Title"
        }
    }
}
```

Given the example below, the value "com2024" would be valid.

```json
"id": {
    "param": {
        "withForum": "or2024"
    }
}
```

* inGroup: valid values are group IDs that are members of the group ID specified in inGroup.

Suppose we have the following group (this is a simplified version of the group):

```json
{
    "id": "OpenReview.net/2022/Conference/Reviewers",
    "readers": ["OpenReview/2022/Conference"],
    "writers": ["OpenReview/2022/Conference"],
    "signatures": ["OpenReview/2022/Conference"],
    "members": ["~Some_Reviewer1", "~Some_Reviewer2"]
}
```

Given the example below, the value "\~Some\_Reviewer1" would be valid.

```json
"tail": {
    "param": {
        "type": "profile",
        "inGroup": "OpenReview.net/2022/Conference/Reviewers"
    }
}
```

### Value Range or Domain Specifiers

* minLength: valid values are strings that have at least the length specified in minLength. In the example below, the values "title" and "long title" are valid.

```json
"some_field": {
    "param": {
        "type": "string",
        "minLength": 5
    }
}
```

* maxLength: valid values are strings that have at most the length specified in maxLength. In the example below, the values "title" and "abc" are valid.

```json
"some_field": {
    "param": {
        "type": "string",
        "maxLength": 5
    }
}
```

* maxSize: specifies the maximum size that a file can have in megabytes. It must be used with type file. In the example below
* extensions: specifies the valid types with the extension of the file. The extensions cannot have dots, which means that files that have the extension tar.gz are not supported.

```json
"supplemtary_material": {
    "param": {
        "type": "file",
        "maxSize": 5,
        "extensions": ["pdf", "zip"]
    }
}
```

* minimum: valid values are numbers (floats or integers) that have at least the value specified in minimum. In the example below, 1 and 6000 are valid values.&#x20;

```json
"some_number": {
    "param": {
        "type": "integer",
        "minimum": 1
    }
}
```

* maximum: valid values are numbers (floats or integers) that have at most the value specified in maximum. In the example below, 20.5 and 16.7 are valid values.

```json
"some_number": {
    "param": {
        "type": "float",
        "maximum": 20.5
    }
}
```

### Field attributes

* type: define **** the type value that field is going to have.

{% hint style="info" %}
The field type is mandatory in all the fields whose type cannot be determined by the name of the field. Note how in the example below the field invitation/edit/signatures defines param/regex without a type keyword. This is because we know the type of the field signatures. We know is of type groups\[], so there is no need to define it. On the other hand, the field invitation/edit/note/content/title/value has to define the type since the fields inside content can be anything.
{% endhint %}

```json
"invitation": {
    "id": "OpenReview.net/2022/Conference/-/Submission",
    "signatures": [ "~Super_User1" ],
    "readers": [ "~" ],
    "writers": [ "groupId" ],
    "invitees": [ "~" ],
    "edit": {
        "signatures": { "param": { "regex": ".*" } },
        "readers": [ "groupId" ],
        "writers": [ "groupId" ],
        "note": {
            "signatures": [ "~Super_User1" ],
            "readers": [ "groupId" ],
            "writers": [ "groupId" ],
            "content": {
                "title": {
                    "value": { "param": { "type": "string", "regex": ".*" } }
                }
            }
        }
    }
}
```

Allowed types:

* string
* integer
* boolean&#x20;
* date&#x20;
* file
* profile
* group
* note

{% hint style="info" %}
The field types profile, group, and note ask for an ID, not the entire object.
{% endhint %}

Square brackets can be appended to each type to expect an array of values, for example:

```json
"keywords": {
    "type" "string[]"
}
```

type can be an optional property in some cases where it is known which type is expected, for example: readers, writers and signatories of a note.

* optional: boolean value that indicates if the value is optional or mandatory.&#x20;
* deletable: boolean value that indicates if the value can be deleted.

{% hint style="info" %}
The fields optional and deletable can be used at the same time or omitted. Their behavior is defined below:
{% endhint %}

| Optional  | Deletable | Behavior                                 |
| --------- | --------- | ---------------------------------------- |
| True      | True      | Field is optional and can be deleted     |
| True      | False     | Field can be added but not deleted       |
| False     | True      | Undefined                                |
| False     | False     | Field is mandatory and cannot be deleted |
| True      | undefined | Field can be added but not deleted       |
| undefined | True      | Field is optional and can be deleted     |
| undefined | undefined | Field is mandatory and cannot be deleted |

### Representation specifiers

{% hint style="info" %}
Representation specifiers are not used for validation, they are only used to correctly display the data in the UI. They are not mandatory and in case they are not present the UI will have default values for each field.
{% endhint %}

* order: define the order in which the field is going to be rendered in the form.
* description: short text to explain the expected value.
* input: type of widget to use when requesting data.
  * text
  * select
  * checkbox
  * textarea
  * radio
* default: value that is going to be shown when the widget is initialized.
* markdown: boolean value that enables the use of markdown.
* scroll: boolean value that specifies whether a textarea input will be scrollable.







****