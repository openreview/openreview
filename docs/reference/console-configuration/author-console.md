# Author Console

### AuthorConsoleConfig : `Object`

AuthorConsole config doc

**Kind**: global typedef\
**Properties**

| Name                    | Type                                             | Default      | Description                       |
| ----------------------- | ------------------------------------------------ | ------------ | --------------------------------- |
| header                  | `Object`                                         |              | mandatory but can be empty object |
| apiVersion              | `1` \| `2`                                       |              | mandatory                         |
| venueId                 | `string`                                         |              | mandatory                         |
| submissionId            | `string`                                         |              | mandatory                         |
| authorSubmissionField   | `string`                                         |              | mandatory                         |
| officialReviewName      | `string`                                         |              | mandatory                         |
| decisionName            | `string`                                         | `"Decision"` | optional                          |
| reviewRatingName        | `string` \| `Array.<string>` \| `Array.<object>` |              | mandatory                         |
| reviewConfidenceName    | `string`                                         |              | mandatory                         |
| authorName              | `string`                                         |              | mandatory                         |
| submissionName          | `string`                                         |              | mandatory                         |
| showAuthorProfileStatus | `boolean`                                        |              | optional                          |
| blindSubmissionId       | `string`                                         |              | optional                          |
| showIEEECopyright       | `boolean`                                        |              | optional                          |
| IEEEPublicationTitle    | `string`                                         |              | optional                          |
| IEEEArtSourceCode       | `number`                                         |              | optional                          |

* AuthorConsoleConfig : `Object`
  * .header : `Object`
  * .apiVersion : `1` | `2`
  * .venueId : `string`
  * .submissionId : `string`
  * .authorSubmissionField : `string`
  * .officialReviewName : `string`
  * .decisionName : `string`
  * .reviewRatingName : `string` | `Array.<string>` | `Array.<object>`
  * .reviewConfidenceName : `string`
  * .authorName : `string`
  * .submissionName : `string`
  * .showAuthorProfileStatus : `boolean`
  * ~~.blindSubmissionId : `string`~~
  * .showIEEECopyright : `boolean`
  * .IEEEPublicationTitle : `string`
  * .IEEEArtSourceCode : `number`

#### AuthorConsoleConfig.header : `Object`

Page header. Contains two string fields: "title" and "instructions" (markdown supported).

**Kind**: static property of `AuthorConsoleConfig`\
**Default**: `no default value`\
**Example**

```js
{
  "header": {
    "title": "Some conference",
    "instructions": "some **instructions**"
  }
}
```

#### AuthorConsoleConfig.apiVersion : `1` | `2`

API version to request. use 2 for new venues.

**Kind**: static property of `AuthorConsoleConfig`\
**Default**: `no default value`\
**Example**

```js
{ "apiVersion": 2 }
```

#### AuthorConsoleConfig.venueId : `string`

The id of the venue group, usually should be value of domain.id

**Kind**: static property of `AuthorConsoleConfig`\
**Default**: `"no default value"`\
**Example**

```js
{ "venueId": "ICLR.cc/202X/Conference" }
```

#### AuthorConsoleConfig.submissionId : `string`

The invitation id of submissions, used to get the notes for display in console.

**Kind**: static property of `AuthorConsoleConfig`\
**Default**: `"no default value"`\
**Example**

```js
{ "submissionId": "ICLR.cc/20XX/Conference/-/Submission" }
```

#### AuthorConsoleConfig.authorSubmissionField : `string`

field name in the submission note that represents the id of author. used to filter notes that current user is an author of.

**Kind**: static property of `AuthorConsoleConfig`\
**Default**: `"no default value"`\
**Example**

```js
{ "authorSubmissionField": "content.authorids" }
```

#### AuthorConsoleConfig.officialReviewName : `string`

name of the offical review in official review invitation, used for header display and for composing the official review invitation id for filtering

**Kind**: static property of `AuthorConsoleConfig`\
**Default**: `"no default value"`\
**Example**

```js
{ "officialReviewName": "Official_Review" }
```

#### AuthorConsoleConfig.decisionName : `string`

name of the decision field in decision note and name of decision invitation. used to construct decision invitation id to find the decision note and to display the paper decision.

**Kind**: static property of `AuthorConsoleConfig`\
**Default**: `"'Decision'"`\
**Example**

```js
{ "decisionName": "Decision" }
```

#### AuthorConsoleConfig.reviewRatingName : `string` | `Array.<string>` | `Array.<object>`

denotes the rating field in review. which is parsed and used for min/max/avg calculation and display.

**Kind**: static property of `AuthorConsoleConfig`\
**Default**: `"no default value"`\
**Example** _(string shows single rating)_

```js
{ "reviewRatingName": "rating" }
```

**Example** _(string array shows multiple ratings)_

```js
{ "reviewRatingName": ["soundness","excitement","reproducibility"] }
```

**Example** _(object array/mixed shows multiple ratings with fallback options the following config would show 2 ratings: "overall\_rating" and "overall\_recommendation" for "overall\_rating", it's value will be final\_rating field, when final\_rating field is not available, it will take the next available value defined in the array, in this example it will take "preliminary\_rating")_

```js
{
 "reviewRatingName": [
   {
     "overall_rating": [
       "final_rating",
       "preliminary_rating"
     ]
   },
   "overall_recommendation",
 ]
```

#### AuthorConsoleConfig.reviewConfidenceName : `string`

name of the decision note. used to construct decision invitation id to find the decision note and to display the paper decision.

**Kind**: static property of `AuthorConsoleConfig`\
**Default**: `"no default value"`\
**Example**

```js
{ "decisionName": "Decision" }
```

#### AuthorConsoleConfig.authorName : `string`

used to construct referrer link back to author console and to filtering invitations for author in tasks tab

**Kind**: static property of `AuthorConsoleConfig`\
**Default**: `"no default value"`\
**Example**

```js
{ "authorName": "Authors" }
```

#### AuthorConsoleConfig.submissionName : `string`

use to construct referrer link/review invitation id/decision invitation id and text display

**Kind**: static property of `AuthorConsoleConfig`\
**Default**: `"no default value"`\
**Example**

```js
{ "submissionName": "Submission" }
```

#### AuthorConsoleConfig.showAuthorProfileStatus : `boolean`

use to control whether to load author profiles. the info is used to display author activation status

**Kind**: static property of `AuthorConsoleConfig`\
**Default**: `no default value but is compared against false so equivalent to true if not provided`\
**Example**

```js
{ "showAuthorProfileStatus": undefined }
```

#### ~~AuthorConsoleConfig.blindSubmissionId : `string`~~

_**only for v1. used to tell whether the note is a bline submission so that author name can be retrieved from original submission**_

**Kind**: static property of `AuthorConsoleConfig`\
**Default**: `"no default value"`<br>

#### AuthorConsoleConfig.showIEEECopyright : `boolean`

used to control whether to show link to IEEE copyright form. used together with IEEEPublicationTitle and IEEEArtSourceCode

**Kind**: static property of `AuthorConsoleConfig`\
**Default**: `no default value, by default the link to IEEE copyright form is hidden`\
**Example**

```js
{ "showIEEECopyright": true }
```

#### AuthorConsoleConfig.IEEEPublicationTitle : `string`

a string assigned by IEEE copyright system. must be exact match with the value assigned. used together with showIEEECopyright and IEEEArtSourceCode

**Kind**: static property of `AuthorConsoleConfig`\
**Default**: `"no default value"`\
**Example**

```js
{ "IEEEPublicationTitle": "20XX The International Conference on Learning Representations (ICLR)" }
```

#### AuthorConsoleConfig.IEEEArtSourceCode : `number`

a number assigned by IEEE copyright system. must be exact match with the value assigned. used together with showIEEECopyright and IEEEPublicationTitle

**Kind**: static property of `AuthorConsoleConfig`\
**Default**: `no default value`\
**Example**

```js
{ "IEEEArtSourceCode": 12345 }
```
