# Program Chair Console

### ProgramChairConsoleConfig : `Object`

Program Chair Console config doc

**Kind**: global typedef\
**Properties**

| Name                                             | Type                                             | Description                       |
| ------------------------------------------------ | ------------------------------------------------ | --------------------------------- |
| header                                           | `Object`                                         | mandatory but can be empty object |
| entity                                           | `Object`                                         | mandatory                         |
| venueId                                          | `string`                                         | mandatory                         |
| reviewersId                                      | `string`                                         | mandatory                         |
| programChairsId                                  | `string`                                         | mandatory                         |
| authorsId                                        | `string`                                         | mandatory                         |
| paperReviewsCompleteThreshold                    | `number`                                         | mandatory                         |
| submissionId                                     | `string`                                         | mandatory                         |
| submissionVenueId                                | `string`                                         | mandatory                         |
| officialReviewName                               | `string`                                         | mandatory                         |
| commentName                                      | `string`                                         | mandatory                         |
| anonReviewerName                                 | `string`                                         | mandatory                         |
| shortPhrase                                      | `string`                                         | mandatory                         |
| enableQuerySearch                                | `boolean`                                        | mandatory                         |
| submissionName                                   | `string`                                         | mandatory                         |
| areaChairsId                                     | `string`                                         | optional                          |
| seniorAreaChairsId                               | `string`                                         | optional                          |
| bidName                                          | `string`                                         | optional                          |
| recommendationName                               | `string`                                         | optional                          |
| metaReviewRecommendationName                     | `string`                                         | optional                          |
| additionalMetaReviewFields                       | `Array.<string>`                                 | optional                          |
| requestFormId                                    | `string`                                         | optional                          |
| withdrawnVenueId                                 | `string`                                         | optional                          |
| deskRejectedVenueId                              | `string`                                         | optional                          |
| officialMetaReviewName                           | `string`                                         | optional                          |
| decisionName                                     | `string`                                         | optional                          |
| anonAreaChairName                                | `string`                                         | optional                          |
| reviewerName                                     | `string`                                         | optional                          |
| areaChairName                                    | `string`                                         | optional                          |
| seniorAreaChairName                              | `string`                                         | optional                          |
| secondaryAreaChairName                           | `string`                                         | optional                          |
| secondaryAnonAreaChairName                       | `string`                                         | optional                          |
| scoresName                                       | `string`                                         | optional                          |
| reviewRatingName                                 | `string` \| `Array.<string>` \| `Array.<object>` | optional                          |
| reviewConfidenceName                             | `string`                                         | optional                          |
| recruitmentName                                  | `string`                                         | optional                          |
| paperStatusExportColumns                         | `Array.<Object>`                                 | optional                          |
| reviewerStatusExportColumns                      | `Array.<Object>`                                 | optional                          |
| areaChairStatusExportColumns                     | `Array.<Object>`                                 | optional                          |
| customStageInvitations                           | `Array.<Object>`                                 | optional                          |
| assignmentUrls                                   | `Object`                                         | optional                          |
| emailReplyTo                                     | `string`                                         | optional                          |
| reviewerEmailFuncs                               | `Array.<Object>`                                 | optional                          |
| acEmailFuncs                                     | `Array.<Object>`                                 | optional                          |
| sacEmailFuncs                                    | `Array.<Object>`                                 | optional                          |
| submissionContentFields                          | `Array.<Object>`                                 | optional                          |
| sacDirectPaperAssignment                         | `boolean`                                        | optional                          |
| propertiesAllowed                                | `Object`                                         | optional                          |
| areaChairStatusPropertiesAllowed                 | `Object`                                         | optional                          |
| sacStatuspropertiesAllowed                       | `Object`                                         | optional                          |
| filterOperators                                  | `Array.<string>`                                 | optional                          |
| messageReviewersInvitationId                     | `string`                                         | optional                          |
| messageAreaChairsInvitationId                    | `string`                                         | optional                          |
| messageSeniorAreaChairsInvitationId              | `string`                                         | optional                          |
| messageSubmissionReviewersInvitationId           | `string`                                         | optional                          |
| messageSubmissionAreaChairsInvitationId          | `string`                                         | optional                          |
| messageSubmissionSecondaryAreaChairsInvitationId | `string`                                         | optional                          |
| preferredEmailInvitationId                       | `string`                                         | optional                          |
| ithenticateInvitationId                          | `string`                                         | optional                          |
| displayReplyInvitations                          | `Array.<Object>`                                 | optional                          |
| metaReviewAgreementConfig                        | `Object`                                         | optional                          |
| useCache                                         | `boolean`                                        | optional                          |
| ethicsReviewersName                              | `string`                                         | optional                          |
| ethicsChairsName                                 | `string`                                         | optional                          |
| domainContent                                    | `Object`                                         | optional                          |

* ProgramChairConsoleConfig : `Object`
  * .header : `Object`
  * .entity : `Object`
  * .venueId : `string`
  * .submissionId : `string`
  * .submissionName : `string`
  * .paperReviewsCompleteThreshold : `number`
  * .submissionVenueId : `string`
  * .requestFormId : `string`
  * .withdrawnVenueId : `string`
  * .deskRejectedVenueId : `string`
  * .reviewersId : `string`
  * .reviewerName : `string`
  * .anonReviewerName : `string`
  * .areaChairsId : `string`
  * .areaChairName : `string`
  * .anonAreaChairName : `string`
  * .seniorAreaChairsId : `string`
  * .seniorAreaChairName : `string`
  * .secondaryAreaChairName : `string`
  * .secondaryAnonAreaChairName : `string`
  * .programChairsId : `string`
  * .authorsId : `string`
  * .officialReviewName : `string`
  * .officialMetaReviewName : `string`
  * .commentName : `string`
  * .decisionName : `string`
  * .bidName : `string`
  * .recommendationName : `string`
  * .scoresName : `string`
  * .metaReviewRecommendationName : `string`
  * .additionalMetaReviewFields : `Array.<string>`
  * .reviewRatingName : `string` | `Array.<string>` | `Array.<object>`
  * .reviewConfidenceName : `string`
  * .enableQuerySearch : `boolean`
  * .shortPhrase : `string`
  * .recruitmentName : `string`
  * .paperStatusExportColumns : `Array.<Object>`
  * .reviewerStatusExportColumns : `Array.<Object>`
  * .areaChairStatusExportColumns : `Array.<Object>`
  * .propertiesAllowed : `Object`
  * .areaChairStatusPropertiesAllowed : `Object`
  * .sacStatuspropertiesAllowed : `Object`
  * .filterOperators : `Array.<string>`
  * .customStageInvitations : `Array.<Object>`
  * .metaReviewAgreementConfig : `Object`
  * .assignmentUrls : `Object`
  * .submissionContentFields : `Array.<Object>`
  * .sacDirectPaperAssignment : `boolean`
  * .messageReviewersInvitationId : `string`
  * .messageAreaChairsInvitationId : `string`
  * .messageSeniorAreaChairsInvitationId : `string`
  * .messageSubmissionReviewersInvitationId : `string`
  * .messageSubmissionAreaChairsInvitationId : `string`
  * .messageSubmissionSecondaryAreaChairsInvitationId : `string`
  * .preferredEmailInvitationId : `string`
  * .emailReplyTo : `string`
  * .reviewerEmailFuncs : `Array.<Object>`
  * .acEmailFuncs : `Array.<Object>`
  * .sacEmailFuncs : `Array.<Object>`
  * .ithenticateInvitationId : `string`
  * .displayReplyInvitations : `Array.<Object>`
  * .useCache : `boolean`
  * .ethicsReviewersName : `string`
  * .ethicsChairsName : `string`
  * .domainContent : `Object`

#### ProgramChairConsoleConfig.header : `Object`

Page header object with "title" and "instructions" (markdown supported).

**Kind**: static property of `ProgramChairConsoleConfig`\
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

#### ProgramChairConsoleConfig.entity : `Object`

Venue group entity. Required for loading and permission checks.

**Kind**: static property of `ProgramChairConsoleConfig`\
**Default**: `no default value`\
**Example**

```js
{ "entity": { "id": "ICLR.cc/202X/Conference" } }
```

#### ProgramChairConsoleConfig.venueId : `string`

Venue group id used as base id/domain for links and API filters.

**Kind**: static property of `ProgramChairConsoleConfig`\
**Default**: `"no default value"`\
**Example**

```js
{ "venueId": "ICLR.cc/202X/Conference" }
```

#### ProgramChairConsoleConfig.submissionId : `string`

Submission invitation id used to load submissions shown in the console.

**Kind**: static property of `ProgramChairConsoleConfig`\
**Default**: `"no default value"`\
**Example**

```js
{ "submissionId": "ICLR.cc/202X/Conference/-/Submission" }
```

#### ProgramChairConsoleConfig.submissionName : `string`

Submission label used for tab names, invitation/group ids and links.

**Kind**: static property of `ProgramChairConsoleConfig`\
**Default**: `"no default value"`\
**Example**

```js
{ "submissionName": "Submission" }
```

#### ProgramChairConsoleConfig.paperReviewsCompleteThreshold : `number`

Threshold used for overview paper progress (papers at or above threshold are considered complete).

**Kind**: static property of `ProgramChairConsoleConfig`\
**Default**: `no default value`\
**Example**

```js
{ "paperReviewsCompleteThreshold": 3 }
```

#### ProgramChairConsoleConfig.submissionVenueId : `string`

Venue id value for active submissions under review. Used in reminder/status filtering.

**Kind**: static property of `ProgramChairConsoleConfig`\
**Default**: `"no default value"`\
**Example**

```js
{ "submissionVenueId": "ICLR.cc/202X/Conference/Submission" }
```

#### ProgramChairConsoleConfig.requestFormId : `string`

Request form note id used for overview timeline/details links. If missing, bottom part of overview tab will not be shown.

**Kind**: static property of `ProgramChairConsoleConfig`\
**Default**: `"no default value"`\
**Example**

```js
{ "requestFormId": "u4QodE3u4r" }
```

#### ProgramChairConsoleConfig.withdrawnVenueId : `string`

Venue id value that marks withdrawn submissions.

**Kind**: static property of `ProgramChairConsoleConfig`\
**Default**: `"no default value"`\
**Example**

```js
{ "withdrawnVenueId": "ICLR.cc/202X/Conference/Withdrawn_Submission" }
```

#### ProgramChairConsoleConfig.deskRejectedVenueId : `string`

Venue id value that marks desk-rejected submissions.

**Kind**: static property of `ProgramChairConsoleConfig`\
**Default**: `"no default value"`\
**Example**

```js
{ "deskRejectedVenueId": "ICLR.cc/202X/Conference/Desk_Rejected_Submission" }
```

#### ProgramChairConsoleConfig.reviewersId : `string`

Reviewer group id. Used for reviewer invitations, assignments, bids and messaging.

**Kind**: static property of `ProgramChairConsoleConfig`\
**Default**: `"no default value"`\
**Example**

```js
{ "reviewersId": "ICLR.cc/202X/Conference/Reviewers" }
```

#### ProgramChairConsoleConfig.reviewerName : `string`

Reviewer role label used to identify per-paper role groups and UI labels.

**Kind**: static property of `ProgramChairConsoleConfig`\
**Default**: `"'Reviewers'"`\
**Example**

```js
{ "reviewerName": "Reviewers" }
```

#### ProgramChairConsoleConfig.anonReviewerName : `string`

Prefix of anonymous reviewer groups used to map reviews to assigned reviewers.

**Kind**: static property of `ProgramChairConsoleConfig`\
**Default**: `"no default value"`\
**Example**

```js
{ "anonReviewerName": "Reviewer_" }
```

#### ProgramChairConsoleConfig.areaChairsId : `string`

AC group id. Enables AC timeline/stats, AC status tab and AC messaging.

**Kind**: static property of `ProgramChairConsoleConfig`\
**Default**: `"no default value"`\
**Example**

```js
{ "areaChairsId": "ICLR.cc/202X/Conference/Area_Chairs" }
```

#### ProgramChairConsoleConfig.areaChairName : `string`

AC role label used to identify per-paper AC groups and display labels.

**Kind**: static property of `ProgramChairConsoleConfig`\
**Default**: `"'Area_Chairs'"`\
**Example**

```js
{ "areaChairName": "Area_Chairs" }
```

#### ProgramChairConsoleConfig.anonAreaChairName : `string`

Prefix of anonymous AC groups used to map meta reviews to assigned ACs.

**Kind**: static property of `ProgramChairConsoleConfig`\
**Default**: `"no default value"`\
**Example**

```js
{ "anonAreaChairName": "Area_Chair_" }
```

#### ProgramChairConsoleConfig.seniorAreaChairsId : `string`

SAC group id. Enables SAC timeline/stats, SAC status tab and SAC messaging.

**Kind**: static property of `ProgramChairConsoleConfig`\
**Default**: `"no default value"`\
**Example**

```js
{ "seniorAreaChairsId": "ICLR.cc/202X/Conference/Senior_Area_Chairs" }
```

#### ProgramChairConsoleConfig.seniorAreaChairName : `string`

SAC role label used to identify per-paper SAC groups and display labels.

**Kind**: static property of `ProgramChairConsoleConfig`\
**Default**: `"'Senior_Area_Chairs'"`\
**Example**

```js
{ "seniorAreaChairName": "Senior_Area_Chairs" }
```

#### ProgramChairConsoleConfig.secondaryAreaChairName : `string`

Secondary AC role label used for assignment/status/messaging for secondary ACs (secondary AC email support in PR #2208).

**Kind**: static property of `ProgramChairConsoleConfig`\
**Default**: `"no default value"`\
**Example**

```js
{ "secondaryAreaChairName": "Secondary_Area_Chairs" }
```

#### ProgramChairConsoleConfig.secondaryAnonAreaChairName : `string`

Prefix for anonymous secondary AC groups.

**Kind**: static property of `ProgramChairConsoleConfig`\
**Default**: `"no default value"`\
**Example**

```js
{ "secondaryAnonAreaChairName": "Secondary_Area_Chair_" }
```

#### ProgramChairConsoleConfig.programChairsId : `string`

Program chairs group id used in overview links.

**Kind**: static property of `ProgramChairConsoleConfig`\
**Default**: `"no default value"`\
**Example**

```js
{ "programChairsId": "ICLR.cc/202X/Conference/Program_Chairs" }
```

#### ProgramChairConsoleConfig.authorsId : `string`

Authors group id used in overview links and counts.

**Kind**: static property of `ProgramChairConsoleConfig`\
**Default**: `"no default value"`\
**Example**

```js
{ "authorsId": "ICLR.cc/202X/Conference/Authors" }
```

#### ProgramChairConsoleConfig.officialReviewName : `string`

Official review invitation name used for review parsing, stats and links.

**Kind**: static property of `ProgramChairConsoleConfig`\
**Default**: `"no default value"`\
**Example**

```js
{ "officialReviewName": "Official_Review" }
```

#### ProgramChairConsoleConfig.officialMetaReviewName : `string`

Meta-review invitation name used for status/statistics/link generation.

**Kind**: static property of `ProgramChairConsoleConfig`\
**Default**: `"'Meta_Review'"`\
**Example**

```js
{ "officialMetaReviewName": "Meta_Review" }
```

#### ProgramChairConsoleConfig.commentName : `string`

Comment invitation name used in overview timeline display.

**Kind**: static property of `ProgramChairConsoleConfig`\
**Default**: `"no default value"`\
**Example**

```js
{ "commentName": "Official_Comment" }
```

#### ProgramChairConsoleConfig.decisionName : `string`

Decision invitation name used to parse/display per-submission decisions. Made optional in PR #1232.

**Kind**: static property of `ProgramChairConsoleConfig`\
**Default**: `"'Decision'"`\
**Example**

```js
{ "decisionName": "Decision" }
```

#### ProgramChairConsoleConfig.bidName : `string`

Bid invitation suffix used for reviewer/AC/SAC bidding data and links. This config is optional since PR #1729.

**Kind**: static property of `ProgramChairConsoleConfig`\
**Default**: `"no default value"`\
**Example**

```js
{ "bidName": "Bid" }
```

#### ProgramChairConsoleConfig.recommendationName : `string`

Reviewer recommendation invitation suffix used for AC recommendation progress.

**Kind**: static property of `ProgramChairConsoleConfig`\
**Default**: `"no default value"`\
**Example**

```js
{ "recommendationName": "Recommendation" }
```

#### ProgramChairConsoleConfig.scoresName : `string`

Score invitation suffix used in edge browser links for bids/recommendations.

**Kind**: static property of `ProgramChairConsoleConfig`\
**Default**: `"no default value"`\
**Example**

```js
{ "scoresName": "Affinity_Score" }
```

#### ProgramChairConsoleConfig.metaReviewRecommendationName : `string`

Recommendation field key in meta-review content.

**Kind**: static property of `ProgramChairConsoleConfig`\
**Default**: `"'recommendation'"`\
**Example**

```js
{ "metaReviewRecommendationName": "recommendation" }
```

#### ProgramChairConsoleConfig.additionalMetaReviewFields : `Array.<string>`

Extra meta-review fields added to table/search/export and status views (feature added in PR #1828).

**Kind**: static property of `ProgramChairConsoleConfig`\
**Default**: `[]`\
**Example**

```js
{ "additionalMetaReviewFields": ["final_recommendation"] }
```

#### ProgramChairConsoleConfig.reviewRatingName : `string` | `Array.<string>` | `Array.<object>`

Review rating field config. Supports string, string\[] and object\[] with fallback fields (fallback support added in PR #1808).

**Kind**: static property of `ProgramChairConsoleConfig`\
**Default**: `"no default value"`\
**Example** _(single field)_

```js
{ "reviewRatingName": "rating" }
```

**Example** _(multiple fields)_

```js
{ "reviewRatingName": ["soundness", "overall_recommendation"] }
```

**Example** _(fallback mapping)_

```js
{
  "reviewRatingName": [
    { "overall_rating": ["final_rating", "preliminary_rating"] },
    "overall_recommendation"
  ]
}
```

#### ProgramChairConsoleConfig.reviewConfidenceName : `string`

Review confidence field used for confidence stats and export.

**Kind**: static property of `ProgramChairConsoleConfig`\
**Default**: `"no default value"`\
**Example**

```js
{ "reviewConfidenceName": "confidence" }
```

#### ProgramChairConsoleConfig.enableQuerySearch : `boolean`

Enables query-search controls in paper, AC and withdrawn/desk-rejected tabs.

**Kind**: static property of `ProgramChairConsoleConfig`\
**Default**: `no default value`\
**Example**

```js
{ "enableQuerySearch": true }
```

#### ProgramChairConsoleConfig.shortPhrase : `string`

Short venue phrase used in email subject and export file names.

**Kind**: static property of `ProgramChairConsoleConfig`\
**Default**: `"no default value"`\
**Example**

```js
{ "shortPhrase": "ICLR 202X" }
```

#### ProgramChairConsoleConfig.recruitmentName : `string`

Recruitment invitation suffix used in overview timeline.

**Kind**: static property of `ProgramChairConsoleConfig`\
**Default**: `"'Recruitment'"`\
**Example**

```js
{ "recruitmentName": "Recruitment" }
```

#### ProgramChairConsoleConfig.paperStatusExportColumns : `Array.<Object>`

Extra export columns for the paper status table. This is an array of objects, where each object has: `header` (column title) and `getValue` (function body string executed with `row` in scope). getValue can get rather complex and the char escape should be handled carefully.

**Kind**: static property of `ProgramChairConsoleConfig`\
**Default**: `no default value`\
**Example** _(basic exmaple)_

```js
{
  "paperStatusExportColumns": [
    {
      "header": "Submission Type",
      "getValue": "row.note?.content?.submission_type?.value ?? 'N/A'"
    }
  ]
}
```

**Example** _(complex exmaple)_

```js
{
  "paperStatusExportColumns": [
    {
      "header": "SAC Recommendation",
      "getValue": `const SACRecommendationNote = row.note?.details?.replies?.find(p => p.invitations.some(q => q.includes('SAC_Recommendation')));
return SACRecommendationNote ? \`\${SACRecommendationNote.content.decision?.value ?? 'N/A'}\` : 'N/A';`,
     },
    }
  ]
}
```

#### ProgramChairConsoleConfig.reviewerStatusExportColumns : `Array.<Object>`

Extra export columns for reviewer status table.

**Kind**: static property of `ProgramChairConsoleConfig`\
**Default**: `no default value`\
**Example**

```js
{
  "reviewerStatusExportColumns": [
    {
      "header": "OpenReview ID",
      "getValue": "row.reviewerProfileId"
    }
  ]
}
```

#### ProgramChairConsoleConfig.areaChairStatusExportColumns : `Array.<Object>`

Extra export columns for AC/SAC status tables.

**Kind**: static property of `ProgramChairConsoleConfig`\
**Default**: `no default value`\
**Example**

```js
{
  "areaChairStatusExportColumns": [
    {
      "header": "AC GS Scholar",
      "getValue": "row.areaChairProfile?.content?.gscholar ?? 'N/A'"
    }
  ]
}
```

#### ProgramChairConsoleConfig.propertiesAllowed : `Object`

Additional query-search properties for paper status. Supports paths array and function strings.

**Kind**: static property of `ProgramChairConsoleConfig`\
**Default**: `no default value`\
**Example**

```js
{
  "propertiesAllowed": {
    "flagged": ["note.content.flagged_for_ethics_review.value","note.content.flagged_for_something_else.value"],
    "officialReviewCount": `const invitationToCheck="Official_Review";
const officialReviews = row.note?.details?.replies?.filter(reply => (reply.invitations.some(invitation => invitation.includes(invitationToCheck))))
return officialReviews.length;
`
  }
}
```

#### ProgramChairConsoleConfig.areaChairStatusPropertiesAllowed : `Object`

Query-search properties override (instead of addition) for AC status, it also support function string

**Kind**: static property of `ProgramChairConsoleConfig`\
**Default**: `built-in AC defaults`\
**Example**

```js
{
  "areaChairStatusPropertiesAllowed": {
    "name": ["areaChairProfile.preferredName"],
    "numTotalReplyCount": `
            const notesAssigned = row.notes
            const replyCounts = notesAssigned.map(note => note.replyCount ?? 0)
            const totalReplyCount = replyCounts.reduce((sum, count) => sum + count, 0)
            return totalReplyCount
          `,
  }
}
```

#### ProgramChairConsoleConfig.sacStatuspropertiesAllowed : `Object`

Query-search properties override for SAC status (direct paper assignment mode), it also support function string

**Kind**: static property of `ProgramChairConsoleConfig`\
**Default**: `built-in SAC defaults`\
**Example**

```js
{
  "sacStatuspropertiesAllowed": {
    "number": ["number"],
    "name": ["sacProfile.preferredName"],
    "email": ["sacProfile.preferredEmail"],
    "hasSubmissionWithFewerThan3Reviews": `
          const assignedNotes = row.notes
          const hasSubmission = assignedNotes.some(note => (note.officialReviews?.length ?? 0) < 3)
          return hasSubmission ? 'yes' : 'no'
        `
  }
}
```

#### ProgramChairConsoleConfig.filterOperators : `Array.<string>`

Allowed query operators for query-search menu bars.

**Kind**: static property of `ProgramChairConsoleConfig`\
**Default**: `["!=", ">=", "<=", ">", "<", "==", "="]`\
**Example**

```js
{ "filterOperators": ["=", "==", "!=", ">", "<", ">=", "<="] }
```

#### ProgramChairConsoleConfig.customStageInvitations : `Array.<Object>`

Additional stage configs used in overview progress cards and table search/export

**Kind**: static property of `ProgramChairConsoleConfig`\
**Default**: `[]`\
**Example**

```js
{
  "customStageInvitations": [
    {
      "name": "Custom_Stage_Field_One",
      "role": "Area_Chairs",
      "repliesPerSubmission": 1,
      "displayField": "custom_stage_field_one",
      "extraDisplayFields": ["some_field"]
    }
  ]
}
```

#### ProgramChairConsoleConfig.metaReviewAgreementConfig : `Object`

Config for dedicated meta-review agreement stage shown in overview and search/export, similar to customStageInvitations but has no extraDisplayFields. Another difference is metaReviewAgreement checks for reply to meta review instead of forum reply

**Kind**: static property of `ProgramChairConsoleConfig`\
**Default**: `no default value`\
**Example**

```js
{
  "metaReviewAgreementConfig": {
    "name": "Meta_Review_Agreement",
    "role": "Area_Chairs",
    "repliesPerSubmission": 1,
    "displayField": "decision",
    "description": "Agreement stage completion"
  }
}
```

#### ProgramChairConsoleConfig.assignmentUrls : `Object`

Per-role assignment URL configuration for timeline and paper status assignment links.

**Kind**: static property of `ProgramChairConsoleConfig`\
**Default**: `no default value`\
**Example**

```js
{
  "assignmentUrls": {
    "Reviewers": {
      "manualAssignmentUrl": "/edges/browse?...",
      "automaticAssignment": false
    },
    "Area_Chairs": {
      "manualAssignmentUrl": "/edges/browse?...",
      "automaticAssignment": true
    }
  }
}
```

#### ProgramChairConsoleConfig.submissionContentFields : `Array.<Object>`

Adds extra paper-status tabs keyed by submission content fields field: The submission content field that the tab will be based on. The tab will filter based on whether or not this field is present in a submission's content responseInvitations: Array of invitation endings that are used to display replies that were made after, and in response to the submission being flagged reasonInvitations: Array of invitation endings that are used to display replies that were made before, or in parallel, the flag and are the reason that the flag was raised reasonFields: Object that contains content fields that may be in any of the notes that reply to any of the reason invitations. These are the fields and possible values that would cause the process function to flag the submission

**Kind**: static property of `ProgramChairConsoleConfig`\
**Default**: `[]`\
**Example**

```js
{
  "submissionContentFields": [
    {
      "field": "ethics_review_flag",
      "reasonInvitations": ["Ethics_Review"],
      "reasonFields": { "ethics_review_triage": ['Ethics review needed.'] },
      "responseInvitations": ["Ethics_Response"]
    }
  ]
}
```

#### ProgramChairConsoleConfig.sacDirectPaperAssignment : `boolean`

If true, SAC status tab shows direct paper assignment progress instead of SAC-to-AC mapping.

**Kind**: static property of `ProgramChairConsoleConfig`\
**Default**: `no default value`\
**Example**

```js
{ "sacDirectPaperAssignment": true }
```

#### ProgramChairConsoleConfig.messageReviewersInvitationId : `string`

Invitation id used when sending bulk reminders from Reviewer Status tab.

**Kind**: static property of `ProgramChairConsoleConfig`\
**Default**: `"no default value"`\
**Example**

```js
{ "messageReviewersInvitationId": "ICLR.cc/202X/Conference/Reviewers/-/Message" }
```

#### ProgramChairConsoleConfig.messageAreaChairsInvitationId : `string`

Invitation id used when sending bulk reminders from AC Status tab.

**Kind**: static property of `ProgramChairConsoleConfig`\
**Default**: `"no default value"`\
**Example**

```js
{ "messageAreaChairsInvitationId": "ICLR.cc/202X/Conference/Area_Chairs/-/Message" }
```

#### ProgramChairConsoleConfig.messageSeniorAreaChairsInvitationId : `string`

Invitation id used when sending bulk reminders from SAC Status tab.

**Kind**: static property of `ProgramChairConsoleConfig`\
**Default**: `"no default value"`\
**Example**

```js
{ "messageSeniorAreaChairsInvitationId": "ICLR.cc/202X/Conference/Senior_Area_Chairs/-/Message" }
```

#### ProgramChairConsoleConfig.messageSubmissionReviewersInvitationId : `string`

Per-submission reviewer reminder invitation id used by Paper Status messaging modal.

**Kind**: static property of `ProgramChairConsoleConfig`\
**Default**: `"no default value"`\
**Example**

```js
{
  "messageSubmissionReviewersInvitationId": "ICLR.cc/202X/Conference/Submission{number}/-/Message"
}
```

#### ProgramChairConsoleConfig.messageSubmissionAreaChairsInvitationId : `string`

Per-submission AC reminder invitation id used by Paper Status messaging modal (selected-paper AC messaging added in PR #2011).

**Kind**: static property of `ProgramChairConsoleConfig`\
**Default**: `"no default value"`\
**Example**

```js
{
  "messageSubmissionAreaChairsInvitationId": "ICLR.cc/202X/Conference/Submission{number}/Area_Chairs/-/Message"
}
```

#### ProgramChairConsoleConfig.messageSubmissionSecondaryAreaChairsInvitationId : `string`

Per-submission secondary-AC reminder invitation id used by Paper Status messaging modal (secondary AC messaging support in PR #2208).

**Kind**: static property of `ProgramChairConsoleConfig`\
**Default**: `"no default value"`\
**Example**

```js
{
  "messageSubmissionSecondaryAreaChairsInvitationId": "ICLR.cc/202X/Conference/Submission{number}/Secondary_Area_Chairs/-/Message"
}
```

#### ProgramChairConsoleConfig.preferredEmailInvitationId : `string`

Invitation id for preferred-email edges used by profile links and copy-email actions.

**Kind**: static property of `ProgramChairConsoleConfig`\
**Default**: `"no default value"`\
**Example**

```js
{ "preferredEmailInvitationId": "ICLR.cc/202X/Conference/-/Preferred_Email" }
```

#### ProgramChairConsoleConfig.emailReplyTo : `string`

Reply-to address for reminder emails sent from PC console tabs.

**Kind**: static property of `ProgramChairConsoleConfig`\
**Default**: `"no default value"`\
**Example**

```js
{ "emailReplyTo": "pc@conference.org" }
```

#### ProgramChairConsoleConfig.reviewerEmailFuncs : `Array.<Object>`

Extra custom message filter options for the Reviewer Status message dropdown.

**Kind**: static property of `ProgramChairConsoleConfig`\
**Default**: `no default value`\
**Example**

```js
{
  "reviewerEmailFuncs": [
    {
      "label": "Reviewers with Zero Load",
      "filterFunc": `
        var loadNotes = row.reviewerProfile.registrationNotes?.filter(n => n.invitations.some(i => i.includes('Max_Load')));
        return parseInt(loadNotes?.[0]?.content?.maximum_load_this_cycle?.value, 10) == 0 ?? False
        `
    }
  ]
}
```

#### ProgramChairConsoleConfig.acEmailFuncs : `Array.<Object>`

Extra custom message filter options for the Area Chair Status message dropdown, similar to reviewerEmailFuncs.

**Kind**: static property of `ProgramChairConsoleConfig`\
**Default**: `no default value`\
**Example**

```js
{
  "acEmailFuncs": [
    {
      "label": "ACs from custom filter",
      "filterFunc": "return row.numCompletedMetaReviews === 0"
    }
  ]
}
```

#### ProgramChairConsoleConfig.sacEmailFuncs : `Array.<Object>`

Extra custom message filter options for the Senior Area Chair Status message dropdown, similar to reviewerEmailFuncs.

**Kind**: static property of `ProgramChairConsoleConfig`\
**Default**: `no default value`\
**Example**

```js
{
  "sacEmailFuncs": [
    {
      "label": "SACs from custom filter",
      "filterFunc": "return row.notes?.length === 0"
    }
  ]
}
```

#### ProgramChairConsoleConfig.ithenticateInvitationId : `string`

Duplication edge invitation id used to display/export similarity percentages.

**Kind**: static property of `ProgramChairConsoleConfig`\
**Default**: `"no default value"`\
**Example**

```js
{
  "ithenticateInvitationId": "ICLR.cc/202X/Conference/Submission/-/Ithenticate"
}
```

#### ProgramChairConsoleConfig.displayReplyInvitations : `Array.<Object>`

Enables and configures the “Latest Replies” column in Paper Status (added in PR #2237).

**Kind**: static property of `ProgramChairConsoleConfig`\
**Default**: `no default value`\
**Example**

```js
{
  "displayReplyInvitations": [
    {
      "id": "ICLR.cc/202X/Conference/Submission{number}/-/Official_Comment",
      "fields": ["summary", "limitations"]
    },
    {
      "id": "ICLR.cc/202X/Conference/Submission{number}/-/Public_Comment",
      "fields": ["strengths", "suitability"]
    }
  ]
}
```

#### ProgramChairConsoleConfig.useCache : `boolean`

If true, PC console loads/saves data via console cache and shows reload banner.

**Kind**: static property of `ProgramChairConsoleConfig`\
**Default**: `false`\
**Example**

```js
{ "useCache": true }
```

#### ProgramChairConsoleConfig.ethicsReviewersName : `string`

Ethics reviewer role label used in timeline/overview role display.

**Kind**: static property of `ProgramChairConsoleConfig`\
**Default**: `"'Ethics_Reviewers'"`\
**Example**

```js
{ "ethicsReviewersName": "Ethics_Reviewers" }
```

#### ProgramChairConsoleConfig.ethicsChairsName : `string`

Ethics chair role label used in timeline/overview role display.

**Kind**: static property of `ProgramChairConsoleConfig`\
**Default**: `"'Ethics_Chairs'"`\
**Example**

```js
{ "ethicsChairsName": "Ethics_Chairs" }
```

#### ProgramChairConsoleConfig.domainContent : `Object`

Domain-level content fallback used in overview for role setup metadata.

**Kind**: static property of `ProgramChairConsoleConfig`\
**Default**: `no default value`\
**Example**

```js
{
  "domainContent": {
    "reviewer_roles": ["Reviewers"],
    "area_chair_roles": ["Area_Chairs"],
    "senior_area_chair_roles": ["Senior_Area_Chairs"]
  }
}
```
