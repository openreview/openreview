# Specifiers

There are two types of specifiers: Validation Specifiers and Representation Specifiers.

Both specifiers are defined inside the param field. The Representation Specifiers define how the requested data will be rendered in the UI.

Validation Specifiers define valid values that can be assigned to the fields of the objects (Edits, Edges, Tags) that will be created using the Invitation. Most Validation Specifiers outside content do not require a `type` field since its type can be inferred from the field name. For instance, the readers field will always be of type `group[]`. However, other Validation Specifiers need to be defined for the readers field, such as regex.

{% hint style="info" %}
Only the mandatory fields head and tail in an Edge must specify the field type even though they are not inside a content field.
{% endhint %}

Validation Specifiers inside the `content` field will always require a `type`. This is because `content` can have any field name and its valid values need to be specified so that the UI can render them and the backend validate them.

In most cases, you can only have one Validation Specifier. The only exceptions are value range specifiers, such as `minimum`, `maximum`, `minLength` and `maxLength`.

There is no restriction on the amount of Representation Specifiers.

## Value Pattern or Validation Specifier

The `param` should have one of the following specifiers needed to validate the expected value.&#x20;

{% hint style="info" %}
Whenever `param` is present, the user is expected to input a value for that field. Otherwise, the value is considered to be a constant and the user cannot change it.
{% endhint %}

* `const`: restricts the value of the field to whatever it's written in const. When `const` is present, the user does not need to pass the value, since it will be assigned by the Invitation. In the example below, the title value has to be exactly "This is a title".

```json
"title": {
    "value": {
        "param": {
            "type": "string",
            "const": "This is a title"
        }
    }
}
```

{% hint style="info" %}
It is preferred to use the following shorthand whenever possible, which is the same as the example above:

```
"title": { "value": "This is a title" }
```
{% endhint %}

{% hint style="info" %}
Fields that are specified as constants in the Invitation do NOT need to be passed when creating an object (Note Edit, Invitation Edit, Group Edit, Tag, or Edge).
{% endhint %}

* `enum`: valid values must be listed in the enum array. The values in `enum` must be of the same type and should match the type specified in the field type. When the type of `enum` is string its values can be regexes too. In the example below "This is a title" and "This issss b regex" are valid values for title.

```json
"title": {
    "value": {
        "param": {
            "type": "string",
            "enum": ["This is a title", "This is{3,5} [a|b] regex"]
        }
    }
}
```

* `regex`: valid values need to match the specified regex. The type of the field should be string. In the example below, "This asdf title" is a valid value.

{% hint style="info" %}
The maximum allowed value for repetition size is 1000. **For example, the following regular expression is** **invalid** **"\[A-Za-z0-9]{0,1001}"**.&#x20;

Having a large repetition size in regular expressions can potentially lead to performance issues, security vulnerabilities, or even crashes in certain cases. This is because regular expressions with large repetition sizes can be computationally expensive to process, especially when applied to long input strings. The engine needs to check all possible combinations within the specified repetition range, which can result in significant slowdowns and resource consumption. This can lead to poor application performance and sluggish user experiences.
{% endhint %}

```json
"title": {
    "value": {
        "param": {
            "type": "string",
            "regex": "^This .+ title$"
        }
    }
}
```

* `range`: valid values are within the range \[a, b] which means a <= value <= b. The type of the field must be float or integer. In the example below, the values 0, 3, and 10 are valid.

```json
"grade": {
    "value": {
        "param": {
            "type": "integer",
            "range": [0, 10]
        }
    }
}
```

* `withInvitation`: valid values are IDs of entities that have as invitation the value specified in `withInvitation`.

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

* `withVenueid`: valid values are IDs of entities that have as `venueid` the value specified in `withVenueid`.

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

* `withForum`: valid values are IDs of entities that have as forum the value specified in `withForum`.

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

* `inGroup`: valid values are group IDs that are members of the group ID specified in `inGroup`.

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

* `minLength`: valid values are strings that have at least the length specified in `minLength`. In the example below, the values "title" and "long title" are valid.

```json
"some_field": {
    "param": {
        "type": "string",
        "minLength": 5
    }
}
```

* `maxLength`: valid values are strings that have at most the length specified in `maxLength`. In the example below, the values "title" and "abc" are valid.

```json
"some_field": {
    "param": {
        "type": "string",
        "maxLength": 5
    }
}
```

* `maxSize`: specifies the maximum size that a file can have in megabytes. It must be used with type file. In the example below
* `extensions`: specifies the valid types with the extension of the file. The extensions cannot have dots, which means that files that have the extension tar.gz are not supported.

```json
"supplemtary_material": {
    "param": {
        "type": "file",
        "maxSize": 5,
        "extensions": ["pdf", "zip"]
    }
}
```

* `minimum`: valid values are numbers (floats or integers) that have at least the value specified in `minimum`. In the example below, 1 and 6000 are valid values.&#x20;

```json
"some_number": {
    "param": {
        "type": "integer",
        "minimum": 1
    }
}
```

* `maximum`: valid values are numbers (floats or integers) that have at most the value specified in `maximum`. In the example below, 20.5 and 16.7 are valid values.

```json
"some_number": {
    "param": {
        "type": "float",
        "maximum": 20.5
    }
}
```

### Field attributes

* `type`: define the type value that field is going to have.

{% hint style="info" %}
The field `type` is mandatory in all the fields whose type cannot be determined by the name of the field. Note how in the example below the field invitation/edit/signatures defines param/regex without a type keyword. This is because we know the type of the field `signatures`. We know is of type `groups[]`, so there is no need to define it. On the other hand, the field `invitation/edit/note/content/title/value` has to define the type since the fields inside content can be anything.
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

1. string
2. integer
3. boolean&#x20;
4. date&#x20;
5. file
6. profile
7. group
8. note

{% hint style="info" %}
The field types profile, group, and note ask for an ID, not the entire object.
{% endhint %}

Square brackets can be appended to each type to expect an array of values, for example:

```json
"keywords": {
    "type" "string[]"
}
```

{% hint style="info" %}
Not all types can become arrays. The exceptions are `date` and `file`.
{% endhint %}

`type` can be an optional property in some cases where it is known which type is expected, for example: `readers`, `writers` and `signatures` of a note.

* `optional`: boolean value that indicates if the value is optional or mandatory.&#x20;
* `deletable`: boolean value that indicates if the value can be deleted.

{% hint style="info" %}
The fields optional and deletable can be used at the same time or omitted. Their behavior is defined below:
{% endhint %}

<table><thead><tr><th width="154.33333333333331">Optional</th><th width="131">Deletable</th><th>Behavior</th></tr></thead><tbody><tr><td>True</td><td>True</td><td>Field is optional and can be deleted</td></tr><tr><td>True</td><td>False</td><td>Field can be added but not deleted</td></tr><tr><td>False</td><td>True</td><td>Undefined</td></tr><tr><td>False</td><td>False</td><td>Field is mandatory and cannot be deleted</td></tr><tr><td>True</td><td>undefined</td><td>Field can be added but not deleted</td></tr><tr><td>undefined</td><td>True</td><td>Field is optional and can be deleted</td></tr><tr><td>undefined</td><td>undefined</td><td>Field is mandatory and cannot be deleted</td></tr></tbody></table>

## Representation specifiers

{% hint style="info" %}
Representation specifiers are not used for validation, they are only used to correctly display the data in the UI. They are not mandatory and in case they are not present the UI will have default values for each field.
{% endhint %}

* `order`: define the order in which the field is going to be rendered in the form.
* `description`: short text to explain the expected value.
* `input`: type of widget to use when requesting data.
  * text
  * select
  * checkbox
  * textarea
  * radio
* `default`: value that is going to be shown when the widget is initialized.
* `markdown`: boolean value that enables the use of markdown.
* `scroll`: boolean value that specifies whether a textarea input will be scrollable.
