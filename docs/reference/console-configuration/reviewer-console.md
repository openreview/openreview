# Reviewer Console

### ReviewerConsoleConfig : `Object`

Reviewer Console config doc

**Kind**: global typedef\
**Properties**

| Name                        | Type                                             | Description                       |
| --------------------------- | ------------------------------------------------ | --------------------------------- |
| header                      | `Object`                                         | mandatory but can be empty object |
| venueId                     | `string`                                         | mandatory                         |
| reviewerName                | `string`                                         | mandatory                         |
| officialReviewName          | `string`                                         | mandatory                         |
| reviewRatingName            | `string` \| `Array.<string>` \| `Array.<object>` | mandatory                         |
| areaChairName               | `string`                                         | optional                          |
| submissionName              | `string`                                         | mandatory                         |
| submissionInvitationId      | `string`                                         | mandatory                         |
| recruitmentInvitationId     | `string`                                         | mandatory                         |
| customMaxPapersInvitationId | `string`                                         | mandatory                         |
| reviewLoad                  | `string` \| `number`                             | mandatory                         |
| hasPaperRanking             | `boolean`                                        | mandatory                         |
| reviewDisplayFields         | `Array.<string>`                                 | optional                          |

* ReviewerConsoleConfig : `Object`
  * .header : `Object`
  * .venueId : `string`
  * .reviewerName : `string`
  * .officialReviewName : `string`
  * .reviewRatingName : `string` | `Array.<string>` | `Array.<object>`
  * .areaChairName : `string`
  * .submissionName : `string`
  * .submissionInvitationId : `string`
  * .recruitmentInvitationId : `string`
  * .customMaxPapersInvitationId : `string`
  * .reviewLoad : `string` | `number`
  * .hasPaperRanking : `boolean`
  * .reviewDisplayFields : `Array.<string>`

#### ReviewerConsoleConfig.header : `Object`

Page header. Contains two string fields: "title" and "instructions" (markdown supported).

**Kind**: static property of `ReviewerConsoleConfig`\
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

#### ReviewerConsoleConfig.venueId : `string`

Used to construct banner content, referrer link and various group/invitation ids. The value is usually domain.id

**Kind**: static property of `ReviewerConsoleConfig`\
**Default**: `"no default value"`\
**Example**

```js
{ "venueId": "ICLR.cc/202X/Conference" }
```

#### ReviewerConsoleConfig.reviewerName : `string`

Used to construct referrer link, title and for filtering groups

**Kind**: static property of `ReviewerConsoleConfig`\
**Default**: `"no default value"`\
**Example**

```js
{ "reviewerName": "Reviewers" }
```

#### ReviewerConsoleConfig.officialReviewName : `string`

Used to construct official review invitation id

**Kind**: static property of `ReviewerConsoleConfig`\
**Default**: `"no default value"`\
**Example**

```js
{ "officialReviewName": "Official_Review" }
```

#### ReviewerConsoleConfig.reviewRatingName : `string` | `Array.<string>` | `Array.<object>`

Used to get rating value from official review, support string, string array for displaying multiple rating fields and object array which allows custom rating name and fallback values

**Kind**: static property of `ReviewerConsoleConfig`\
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

#### ReviewerConsoleConfig.areaChairName : `string`

Used to construct AC/Anonymous AC group and label. optional for venues that don't have area chairs

**Kind**: static property of `ReviewerConsoleConfig`\
**Default**: `"no default value"`\
**Example**

```js
{ "areaChairName": "Area_Chairs" }
```

#### ReviewerConsoleConfig.submissionName : `string`

Used to filter/construct group id (paper display/tasks), invitation id, header text

**Kind**: static property of `ReviewerConsoleConfig`\
**Default**: `"no default value"`\
**Example**

```js
{ "submissionName": "Submission" }
```

#### ReviewerConsoleConfig.submissionInvitationId : `string`

Notes with the submissionInvitationId will be fetched

**Kind**: static property of `ReviewerConsoleConfig`\
**Default**: `"no default value"`\
**Example**

```js
{ "submissionInvitationId": "ICLR.cc/202X/Conference/-/Submission" }
```

#### ReviewerConsoleConfig.recruitmentInvitationId : `string`

Related to customMaxPapersInvitationId and reviewLoad. The invitation to get recruitment note where custom load is saved (if there's no custom load edge) and custom load is displayed in header

**Kind**: static property of `ReviewerConsoleConfig`\
**Default**: `"no default value"`\
**Example**

```js
{ "recruitmentInvitationId": "ICLR.cc/202X/Conference/Reviewers/-/Recruitment" }
```

#### ReviewerConsoleConfig.customMaxPapersInvitationId : `string`

Related to recruitmentInvitationId and reviewLoad. The invitation to get custom load edge. If edge exist the weight is used as custom load otherwise it will load the recruitment note to read the reduced\_load field.

**Kind**: static property of `ReviewerConsoleConfig`\
**Default**: `"no default value"`\
**Example**

```js
{ "customMaxPapersInvitationId": "ICLR.cc/202X/Conference/Reviewers/-/Custom_Max_Papers" }
```

#### ReviewerConsoleConfig.reviewLoad : `string` | `number`

Related to recruitmentInvitationId and customMaxPapersInvitationId. The default value to display in header when there's no custom load edge or recruitment note

**Kind**: static property of `ReviewerConsoleConfig`\
**Default**: `"no default value"`\
**Example**

```js
{ "reviewLoad": "" }
```

#### ReviewerConsoleConfig.hasPaperRanking : `boolean`

Flag to enable paper ranking (tag fetching and display)

**Kind**: static property of `ReviewerConsoleConfig`\
**Default**: `no default value`\
**Example**

```js
{ "hasPaperRanking": false }
```

#### ReviewerConsoleConfig.reviewDisplayFields : `Array.<string>`

The content fields to display from official review note

**Kind**: static property of `ReviewerConsoleConfig`\
**Default**: `['review']`\
**Example**

```js
{ "reviewDisplayFields": ['review'] }
```
