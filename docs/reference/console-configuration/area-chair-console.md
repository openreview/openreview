# Area Chair Console

### AreaChairConsoleConfig : `Object`

Area Chair Console config doc

**Kind**: global typedef\
**Properties**

| Name                         | Type                                             | Description                       |
| ---------------------------- | ------------------------------------------------ | --------------------------------- |
| header                       | `Object`                                         | mandatory but can be empty object |
| venueId                      | `string`                                         | mandatory                         |
| reviewerAssignment           | `Object`                                         | optional                          |
| submissionInvitationId       | `string`                                         | mandatory                         |
| seniorAreaChairsId           | `string`                                         | optional                          |
| areaChairName                | `string`                                         | mandatory                         |
| secondaryAreaChairName       | `string`                                         | optional                          |
| submissionName               | `string`                                         | mandatory                         |
| officialReviewName           | `string`                                         | mandatory                         |
| reviewRatingName             | `string` \| `Array.<string>` \| `Array.<object>` | mandatory                         |
| reviewConfidenceName         | `string`                                         | optional                          |
| officialMetaReviewName       | `string`                                         | mandatory                         |
| reviewerName                 | `string`                                         | mandatory                         |
| anonReviewerName             | `string`                                         | mandatory                         |
| metaReviewRecommendationName | `string`                                         | optional                          |
| additionalMetaReviewFields   | `Array.<string>`                                 | optional                          |
| shortPhrase                  | `string`                                         | mandatory                         |
| filterOperators              | `Array.<string>`                                 | optional                          |
| propertiesAllowed            | `Array.<Object>`                                 | optional                          |
| enableQuerySearch            | `boolean`                                        | optional                          |
| emailReplyTo                 | `string`                                         | optional                          |
| extraExportColumns           | `Array.<Object>`                                 | optional                          |
| preferredEmailInvitationId   | `string`                                         | optional                          |
| ithenticateInvitationId      | `string`                                         | optional                          |
| extraRoleNames               | `Array.<string>`                                 | optional                          |
| sortOptions                  | `Array.<Object>`                                 | optional                          |
| displayReplyInvitations      | `Array.<Object>`                                 | optional                          |
| customStageInvitations       | `Array.<Object>`                                 | optional                          |

* AreaChairConsoleConfig : `Object`
  * .header : `Object`
  * .venueId : `string`
  * .reviewerAssignment : `Object`
  * .submissionInvitationId : `string`
  * .seniorAreaChairsId : `string`
  * .areaChairName : `string`
  * .secondaryAreaChairName : `string`
  * .submissionName : `string`
  * .officialReviewName : `string`
  * .reviewConfidenceName : `string`
  * .officialMetaReviewName : `string`
  * .reviewerName : `string`
  * .anonReviewerName : `string`
  * .metaReviewRecommendationName : `string`
  * .additionalMetaReviewFields : `Array.<string>`
  * .shortPhrase : `string`
  * .filterOperators : `Array.<string>`
  * .propertiesAllowed : `Array.<object>`
  * .enableQuerySearch : `boolean`
  * .emailReplyTo : `string`
  * .extraExportColumns : `Array.<object>`
  * .preferredEmailInvitationId : `string`
  * .ithenticateInvitationId : `string`
  * .extraRoleNames : `Array.<string>`
  * .sortOptions : `Array.<object>`
  * .displayReplyInvitations : `Array.<object>`
  * .customStageInvitations : `Array.<object>`

#### AreaChairConsoleConfig.header : `Object`

Page header. Contains two string fields: "title" and "instructions" (markdown supported)

**Kind**: static property of `AreaChairConsoleConfig`\
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

#### AreaChairConsoleConfig.venueId : `string`

Used to construct banner content, referrer link and various group/invitation ids. The value is usually domain.id

**Kind**: static property of `AreaChairConsoleConfig`\
**Default**: `"no default value"`\
**Example**

```js
{ "venueId": "ICLR.cc/202X/Conference" }
```

#### AreaChairConsoleConfig.reviewerAssignment : `Object`

An object which can contain the following fields:

* showEdgeBrowserUrl (boolean): a flag to control whether to show link to edge browser to edit reviewer assignments
* proposedAssignmentTitle (string): the title of the proposed assignment config note. if provided, edgeBrowserProposedUrl will be used as the link to the edge browser, otherwise edgeBrowserDeployedUrl will be used
* edgeBrowserProposedUrl (string): the url to edge browser to edit (proposed) assignments. shown in instructions when showEdgeBrowserUrl is true and proposedAssignmentTitle is provided
* edgeBrowserDeployedUrl (string): the url to edge browser to edit deployed assignments. shown in instructions when showEdgeBrowserUrl is true and proposedAssignmentTitle is not provided

**Kind**: static property of `AreaChairConsoleConfig`\
**Default**: `no default value`\
**Example**

```js
{
  "reviewerAssignment": {
    "showEdgeBrowserUrl": true,
    "proposedAssignmentTitle": "Min0-Max3",
    "edgeBrowserProposedUrl": "/edges/browse?start=ICLR.cc/202X/Conference/Area_Chairs/-/Assignment,tail:~Test_User1&traverse=ICLR.cc/202X/Conference/Reviewers/-/Proposed_Assignment,label:undefined&edit=ICLR.cc/202X/Conference/Reviewers/-/Proposed_Assignment,label:undefined;ICLR.cc/202X/Conference/Reviewers/-/Invite_Assignment&browse=ICLR.cc/202X/Conference/Reviewers/-/Aggregate_Score,label:undefined;ICLR.cc/202X/Conference/Reviewers/-/Affinity_Score;ICLR.cc/202X/Conference/Reviewers/-/Bid&maxColumns=2&version=2",
    "edgeBrowserDeployedUrl": "/edges/browse?start=ICLR.cc/202X/Conference/Area_Chairs/-/Assignment,tail:~Test_User1&traverse=ICLR.cc/202X/Conference/Reviewers/-/Assignment&edit=ICLR.cc/202X/Conference/Reviewers/-/Invite_Assignment&browse=ICLR.cc/202X/Conference/Reviewers/-/Affinity_Score;ICLR.cc/202X/Conference/Reviewers/-/Bid;ICLR.cc/202X/Conference/Reviewers/-/Custom_Max_Papers,head:ignore&maxColumns=2&version=2"
  }
}
```

#### AreaChairConsoleConfig.submissionInvitationId : `string`

Used as the invitation to query notes to be displayed in console

**Kind**: static property of `AreaChairConsoleConfig`\
**Default**: `"no default value"`\
**Example**

```js
{ "submissionInvitationId": "ICLR.cc/202X/Conference/-/Submission" }
```

#### AreaChairConsoleConfig.seniorAreaChairsId : `string`

The SAC group id. Used to construct SAC-AC assignment invitation id to query SAC assignment edges and to construct the SAC display text

**Kind**: static property of `AreaChairConsoleConfig`\
**Default**: `"no default value"`\
**Example**

```js
{ "seniorAreaChairsId": "ICLR.cc/202X/Conference/Senior_Area_Chairs" }
```

#### AreaChairConsoleConfig.areaChairName : `string`

The name of ACs group used in group id. Used to identify ACs group anonymous AC group

**Kind**: static property of `AreaChairConsoleConfig`\
**Default**: `"no default value"`\
**Example**

```js
{ "areaChairName": "Area_Chairs" }
```

#### AreaChairConsoleConfig.secondaryAreaChairName : `string`

The name of Secondary ACs group used in group id. Used to identify Secondary ACs group to determine the submission to show in AC triplet(Secondary AC) tab

**Kind**: static property of `AreaChairConsoleConfig`\
**Default**: `"no default value"`\
**Example**

```js
{ "secondaryAreaChairName": "Secondary_Area_Chairs" }
```

#### AreaChairConsoleConfig.submissionName : `string`

Name of submission, used to construct group id, query param, error message etc

**Kind**: static property of `AreaChairConsoleConfig`\
**Default**: `"no default value"`\
**Example**

```js
{ "submissionName": "Submission" }
```

#### AreaChairConsoleConfig.officialReviewName : `string`

Name of official reviews, used in label text and to construct official review invitation id

**Kind**: static property of `AreaChairConsoleConfig`\
**Default**: `"no default value"`\
**Example**

```js
{ "officialReviewName": "Official_Review" }
```

#### AreaChairConsoleConfig.reviewConfidenceName : `string`

Name of confidence field in official review.

**Kind**: static property of `AreaChairConsoleConfig`\
**Default**: `"no default value"`\
**Example**

```js
{ "reviewConfidenceName": "confidence_level" }
```

#### AreaChairConsoleConfig.officialMetaReviewName : `string`

Name of meta review. Used in label text and to construct meta review invitation id.

**Kind**: static property of `AreaChairConsoleConfig`\
**Default**: `"no default value"`\
**Example**

```js
{ "officialMetaReviewName": "Meta_Review" }
```

#### AreaChairConsoleConfig.reviewerName : `string`

Used to construct label text and for filtering reviewers group

**Kind**: static property of `AreaChairConsoleConfig`\
**Default**: `"'Reviewers'"`\
**Example**

```js
{ "reviewerName": "Reviewers" }
```

#### AreaChairConsoleConfig.anonReviewerName : `string`

Used to filter anonymous reviewer groups

**Kind**: static property of `AreaChairConsoleConfig`\
**Default**: `"'Reviewer_'"`\
**Example**

```js
{ "anonReviewerName": "Reviewer_" }
```

#### AreaChairConsoleConfig.metaReviewRecommendationName : `string`

Name of recommendation field in meta review

**Kind**: static property of `AreaChairConsoleConfig`\
**Default**: `"'recommendation'"`\
**Example**

```js
{ "metaReviewRecommendationName": "recommendation" }
```

#### AreaChairConsoleConfig.additionalMetaReviewFields : `Array.<string>`

Additional fields to display from meta review

**Kind**: static property of `AreaChairConsoleConfig`\
**Default**: `[]`\
**Example**

```js
{ "additionalMetaReviewFields": ["final_recommendation"] }
```

#### AreaChairConsoleConfig.shortPhrase : `string`

Used in text when messaging reviewers

**Kind**: static property of `AreaChairConsoleConfig`\
**Default**: `"no default value"`\
**Example**

```js
{ "shortPhrase": "ABCD 20XX" }
```

#### AreaChairConsoleConfig.filterOperators : `Array.<string>`

The query search operator allowed

**Kind**: static property of `AreaChairConsoleConfig`\
**Default**: `no default value set in AC Console, default to ['!=', '>=', '<=', '>', '<', '==', '='] in menu bar`\
**Example**

```js
{ "filterOperators": [">","<"] }
```

#### AreaChairConsoleConfig.propertiesAllowed : `Array.<object>`

Properties allowed in query search apart from default properties defined in menu bar

**Kind**: static property of `AreaChairConsoleConfig`\
**Default**: `no default value`\
**Example**

```js
{
 "propertiesAllowed": [
   {
     track:['note.content.track.value']
   }
 ]
}
```

#### AreaChairConsoleConfig.enableQuerySearch : `boolean`

Controls whether to enable query search in menu bar, if not set or set to false, only basic search is available (filterOperators and propertiesAllowed will be ignored)

**Kind**: static property of `AreaChairConsoleConfig`\
**Default**: `no default value, equivalent to false`\
**Example**

```js
{
 "enableQuerySearch": true
}
```

#### AreaChairConsoleConfig.emailReplyTo : `string`

replyto of the emails sent from console

**Kind**: static property of `AreaChairConsoleConfig`\
**Default**: `"no default value"`\
**Example**

```js
{
 "emailReplyTo": "conference@contact.email"
}
```

#### AreaChairConsoleConfig.extraExportColumns : `Array.<object>`

Extra data in the CSV export. Each object contains a header prop (header in exported csv) and a getValue string which is a function that takes row data as param and read the value required in export

**Kind**: static property of `AreaChairConsoleConfig`\
**Default**: `no default value`\
**Example**

```js
{
 "extraExportColumns": [
   {
     track:['note.content.track.value'],
     getValue: `
        return row.reviewers?.
         map((reviewer) => {
           const review = row.officialReviews?.find(
             (review) => review.anonymousId === reviewer.anonymousId
           );
           return \`\${reviewer.preferredName}:rating-\${review?.rating??'N/A'}|confidence-\${review?.confidence??'N/A'}|first time reviewer-\${review?.content?.first_time_reviewer?.value ?? 'N/A'}\`;
         })
         .join(',')
     `
   }
 ]
}
```

#### AreaChairConsoleConfig.preferredEmailInvitationId : `string`

Invitation id to get preferred email edges to show SAC contact

**Kind**: static property of `AreaChairConsoleConfig`\
**Default**: `"no default value"`\
**Example**

```js
{
 "preferredEmailInvitationId": "conference/202XX/Conference/-/Preferred_Emails"
}
```

#### AreaChairConsoleConfig.ithenticateInvitationId : `string`

Invitation id to get iThenticate edges to show in paper summary

**Kind**: static property of `AreaChairConsoleConfig`\
**Default**: `"no default value"`\
**Example**

```js
{
 "ithenticateInvitationId": 'conference/20XX/Conference/-/iThenticate_Plagiarism_Check'
}
```

#### AreaChairConsoleConfig.extraRoleNames : `Array.<string>`

The role names of the AC to display a task tab for, task of each role apart from the AC role will be displayed in a separate tab

**Kind**: static property of `AreaChairConsoleConfig`\
**Default**: `no default value`\
**Example** _(Two Task tabs will be displayed: Area Chair Tasks and Secondary Area Chair Tasks)_

```js
{
 "extraRoleNames": ["Secondary_Area_Chairs"]
}
```

#### AreaChairConsoleConfig.sortOptions : `Array.<object>`

Custom sort options to be added apart from the default sort options in menu bar sorting dropdown, each object contains: label: the value to be shown in dropdown options, value: a value used as id of the dropdown options, getValue: a string function to calculate the order. getValue function has row data as input param

**Kind**: static property of `AreaChairConsoleConfig`\
**Default**: `no default value`\
**Example** _(Following config add "Confidence Spread" in sorting dropdown)_

```js
{
 "sortOptions": [
   {
     label:'Confidence Spread',
     value:'confidence spread',
     getValue:"return row.reviewProgressData?.confidenceMax - row.reviewProgressData?.confidenceMin"
   }
 ]
}
```

#### AreaChairConsoleConfig.displayReplyInvitations : `Array.<object>`

The invitation id and field of the forum reply to be shown in a "Latest Replies" column. Each object has 2 fields: id: the invitation id to get reply, {number} in the id is replaced with the actual paper number. fields: an array of string specifying the reply field to show

**Kind**: static property of `AreaChairConsoleConfig`\
**Default**: `no default value`\
**Example**

```js
{
 "displayReplyInvitations": [
   {
     id: "abcd.org/20XX/Conference/Submission{number}/-/Official_Review",
     fields: ["summary", "limitations"],
   },
   {
     id: "abcd.org/20XX/Conference/Submission{number}/-/Public_Comment",
     fields: ["strengths", "suitability"],
   }
 ]
}
```

#### AreaChairConsoleConfig.customStageInvitations : `Array.<object>`

config the custom stage replies to be shown under meta review status column. Each object can have 3 fields: name: construct the invitation id to fiter note replies, displayField: the field name to read from the custom stage note, extraDisplayFields: an string array with more fields to show from the custom stage note. Compared to the customStageInvitations config in PC/SAC console, it does not have role or repliesPerSubmission

**Kind**: static property of `AreaChairConsoleConfig`\
**Default**: `no default value`\
**Example**

```js
{
 "customStageInvitations": [
   {
     name:"Meta_Review_Confirmation",
     displayField: "decision",
     extraDisplayFields: ["meta_review_agreement"],
   }
 ]
}
```
