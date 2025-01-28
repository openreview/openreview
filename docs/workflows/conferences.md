---
description: Guide based on ICLR venue
---

# Example Workflow

How to use this document: This document lists the major steps of running a conference venue.  Each step links to relevant documentation that explains it in depth.&#x20;

### Setting up a venue

#### 1. [Submit venue request through the OpenReview Site ](../getting-started/hosting-a-venue-on-openreview/creating-your-venue-instance-submitting-a-venue-request-form.md#submitting-a-venue-request-form)

Fill out the venue request form and choose settings for your venue.

OpenReview will review the request, ask for any necessary clarification, then you will receive an email notifying you that your venue has been deployed.&#x20;

Also see:

[Customizing venue homepage](../how-to-guides/modifying-venue-homepages/how-to-customize-your-venue-homepage.md)

[Customizing submission form](../getting-started/hosting-a-venue-on-openreview/customizing-your-submission-form.md)

#### 2. [Review venue pages](../getting-started/hosting-a-venue-on-openreview/navigating-your-venue-pages.md)

Ensure all PCs have OpenReview accounts associated with the email listed in the venue request form in order to access venue pages.

#### 3. [Change settings of your venue as necessary from the venue request form](../reference/stages/revision.md)

You may edit many settings for your venue through the 'Revision' button of the request form.&#x20;

#### 4. [Recruit Senior Area Chairs, Area Chairs, Reviewers](../how-to-guides/managing-groups/how-to-recruit-and-remind-recruited-reviewers.md)

Recruitment can happen at any point in the workflow. All participants (Area Chairs, Reviewers, etc.) must have an OpenReview account linked to the email used in the recruitment message.

**5. (optional)** [**Create a registration task for Senior Area Chairs, Area Chairs and Reviewers**](../reference/stages/registration-stage.md)

You may choose to add a task for program committee members to remind them to complete their registration.

### Submission Phase

#### 6. Submissions Open

Submissions automatically open on the date/time listed in the venue request form. If no date is given, submissions open as soon as the venue is deployed.

Note: All submitting authors must have an active OpenReview Profile.

#### 7. (Optional) [Abstract Registration Stage](../getting-started/hosting-a-venue-on-openreview/enabling-an-abstract-registration-deadline.md)

Use this if you want to have an abstract deadline ahead of the main submission deadline.

#### 8. Submissions close

Submissions will automatically close on the date specified in the venue request form. To change the submission deadline, see [here](../getting-started/hosting-a-venue-on-openreview/changing-your-submission-deadline.md).

#### 9. Update readers/ hide fields from authors using [Post Submission Stage](../reference/stages/post-submission-stage.md)

### Set up matches between submissions and program committee

Set up matching should be done in the direction Senior Area Chairs-> Area Chairs-> Submissions, then Reviewers should be assigned to submissions. The basic stages of this workflow is to:

1. [Set up matching](../how-to-guides/paper-matching-and-assignment/how-to-do-automatic-assignments/how-to-setup-paper-matching-by-calculating-affinity-scores-and-conflicts.md)
2. (optional)[ Bidding](../reference/stages/bid-stage.md)
3. [Run matching](../how-to-guides/paper-matching-and-assignment/how-to-do-automatic-assignments/how-to-run-a-paper-matching.md)
4. Look at, and potentially [modify](../how-to-guides/paper-matching-and-assignment/how-to-do-automatic-assignments/how-to-modify-the-proposed-assignments.md) proposed assignments
5. [Deploy](../how-to-guides/paper-matching-and-assignment/how-to-do-automatic-assignments/how-to-deploy-the-proposed-assignments.md) assignments
6. [Confirm](../how-to-guides/workflow/how-to-change-who-can-access-submissions-after-the-deadline.md) that assigned program committee are readers of their assigned submissions

Each of these steps needs to be completed for each group being matched (e.g. Area Chairs - Submissions , Reviewers - Submissions)- Described further in steps 11-16 below.

#### 10.  Assign Senior Area Chairs to Area Chairs

Most venues assign SACs to submissions by first assigning them to Area Chairs. Here you may decide whether to have SACs automatically assigned to ACs or [allow them to bid on ACs ](../how-to-guides/workflow/how-to-enable-bidding-for-senior-area-chair-assignment.md)based on affinity scores by following steps 1-5 above.&#x20;

To assign SACs to ACs, choose 'Senior Area Chairs' as the matching group in paper matching setup. Then in the Paper Matching Stage, SACs will be the matching group, and ACs will be in the Submissions field.&#x20;

Note: Matching between SACs and ACs will not calculate conflicts. Instead, the conflicts to the SACs will be transferred to the ACs and calculated at the AC matching stage.

Program Chairs can make reassignments after the proposed assignments are deployed or [undeploy](../how-to-guides/paper-matching-and-assignment/how-to-undo-deployed-assignments.md) assignments.

**It is very important to deploy Senior Area Chair assignments before assigning submissions to Area Chairs to allow conflicts to be transferred successfully.**

If you decide to directly assign Senior Area Chairs to submissions, skip to step 12.&#x20;

#### 11. [Setup Paper Matching between Senior Area Chairs/Area Chairs/Reviewers and submissions](../how-to-guides/paper-matching-and-assignment/how-to-do-automatic-assignments/how-to-setup-paper-matching-by-calculating-affinity-scores-and-conflicts.md)

The next step is to assign the program committee to each submission. If your conference is smaller than 2000 submissions, this can be run by you directly, otherwise please contact OpenReview.

It is very important that all Program Committee members (Senior Area Chairs, Area Chairs, and Reviewers) have a complete OpenReview profile (an active profile with at least one active institution and one publication).  We recommend removing from the committee groups all the profiles that are not complete before running the matching system.

{% hint style="info" %}
To make sure that the program committee can access their assignments, ensure that they are listed as readers in the Submission Readers field of the venue request form, othewise update this value in the Post Submission stage.
{% endhint %}

#### 12. (Optional) Bidding period for Senior Area Chairs, Area Chairs and Reviewers

Program Chairs can optionally ask the Senior Area Chairs, Area Chairs and/or Reviewers to [bid on papers](../reference/stages/bid-stage.md).&#x20;

PCs must make all the existing submissions visible to all the members of the Senior Area Chairs/Area Chair/Reviewers group hiding the PDF, supplementary material and any other fields they don’t want Senior Area Chairs/Area Chair/Reviewers to see by using the [Post Submission ](../reference/stages/post-submission-stage.md)stage.&#x20;

In order for conflicts to be taken into account in paper matching, matching set up (Step 14) must be run before the bid stage.

The bidding console shows the submissions sorted by affinity scores of the logged user and filtering out the ones that are in conflict with the user.

Note: when the conference is large, only sparse scores are uploaded to the system, this means we only keep the first N (400) scores for each user and submission. Users will see at least 400 submissions sorted by their affinity scores. If they want to see other submissions that are not among the top 400 they should use the “search” functionality.

#### 13. [Run matching between Submissions and Senior Area Chairs/Area Chairs/Reviewers](../how-to-guides/paper-matching-and-assignment/how-to-do-automatic-assignments/how-to-run-a-paper-matching.md)

After computing affinity scores and optionally enabling the bidding process, Program Chairs can run the paper matching system to assign Senior Area Chairs/Area Chairs/Reviewers to Submissions. Program Chairs can assign Senior Area Chairs/Area Chairs/Reviewers at different stages because they are independent processes.

#### 14. (optional) Share _Reviewer-Submission_ proposed assignments with Area Chairs to review and edit.

Some venues decide to share the _Reviewer-Submission_ assignments with the Area Chairs to review before releasing them to the Reviewers.&#x20;

In order to do this, Program Chairs need to [deploy](../how-to-guides/paper-matching-and-assignment/how-to-do-automatic-assignments/how-to-deploy-the-proposed-assignments.md) the _Area Chair-Submission_ assignments so that Area Chairs can see their own assigned submissions and choose a matching configuration to share the proposed assignments. Then you can [allow ACs to do reassignment](../how-to-guides/paper-matching-and-assignment/how-to-enable-reviewer-reassignment-for-area-chairs.md).&#x20;

[Area Chairs can make modifications to these assignments and they can optionally invite external reviewers](../how-to-guides/paper-matching-and-assignment/how-to-do-automatic-assignments/how-to-modify-the-proposed-assignments.md). This must be requested to the support team.

#### 15. [Deploy proposed assignments for Senior Area Chairs, Area Chairs and Reviewers](../how-to-guides/paper-matching-and-assignment/how-to-do-automatic-assignments/how-to-deploy-the-proposed-assignments.md)

After the proposed assignments were reviewed by the Program Chairs and/or the Senior Area Chairs/Area Chairs, they can be deployed and be visible to the Reviewers. Deploying assignments doesn’t automatically send emails to the Reviewers- it is recommended that you as PCs notify them.



**16. (optional) Modify assignments**

Deployed assignments can be [edited](../how-to-guides/paper-matching-and-assignment/how-to-do-automatic-assignments/how-to-modify-the-proposed-assignments.md) by the Program Chairs and Senior Area Chairs. Program Chairs can also decide if they want to [allow Area Chairs to make reassignments](../how-to-guides/paper-matching-and-assignment/how-to-enable-reviewer-reassignment-for-area-chairs.md). When a reassignment is done, an email notification is sent to the new Reviewer.

### Review Stage

#### 17. Start review period

You can start the review period through the [Review Stage](../reference/stages/review-stage.md). You may also at this point [customize](../getting-started/customizing-forms.md) the[ default review form](../reference/default-forms/default-review-form.md).

Note: There are two fields with default names “rating” and “confidence” that are used to compute stats in the different consoles. You may modify these fields but make sure to specify the names in the [Rating/Confidence Field Name ](conferences.md#review-stage)field of the Revision Stage.

It is also important to choose what groups a review should be visible to. A common configuration is that reviews are only visible to the assigned Senior Area Chair, Area Chair and Reviewers. You may change the readers of reviews at any time using the [Review Stage.](conferences.md#review-stage)

#### 18. [Review Stage](conferences.md#review-stage) ends

The deadline field of the Review Stage form will be the one shown to reviewers as the advertised deadline. The expiration date in the form (not seen by reviewers) will be the time after which no more reviews can be submitted. After this point, reviewers will need to contact the PCs for any late reviews.

#### 19.[ Release reviews](../how-to-guides/workflow/how-to-release-reviews.md) to the authors and other reviewers

If there are still pending reviews, setting Release Reviews to Authors to "Yes, reviews should be revealed when they are posted to the paper's authors", will release all posted reviews, and later will release pending reviews after they are posted.

### (Optional) Rebuttal Stage

#### 20. Start Rebuttal stage

Usually venues have a rebuttal period where Authors can reply to the Reviewers. In OpenReview, the rebuttal period can start at any time using the [rebuttal stage](../reference/stages/rebuttal-stage.md). They can choose between settings to allow a free number of rebuttal comments or require authors to have one rebuttal per Submission/Review.&#x20;

{% hint style="info" %}
Readers of the rebuttal must match the review readers. PCs can check the review readers selected in the Review Stage, in particular: Release Reviews To Authors and Release Reviews To Authors, and match the rebuttal readers to these options
{% endhint %}



PCs may also use the [comment stage](../reference/stages/comment-stage.md) so that reviewers can optionally reply to the authors and keep threaded discussions.

### (Optional) Submission revision stage

#### 21.  Start [Submission Revision Stage](../reference/stages/submission-revision-stage.md)

You may optionally allow authors to revise their submissions, including limiting which fields can be edited. This stage can be enabled any time after the submission deadline has passed.

### (Optional) Meta-review Stage

#### 22. [Start meta review period](conferences.md#metareview-stage)

When the review period has concluded, the PCs can start the [meta review period](https://docs.openreview.net/reference/stages/meta-review-stage) where the ACs make their recommendations.

#### 23. (optional) Start meta review confirmation period

Use this if you have Senior Area Chairs and want them to review, confirm, or revise the meta review posted by the Area Chair.

#### 24. (optionaI) ACs rate reviews

You may also optionally allow Area Chairs to submit ratings for their reviews (found in [venue request form](../getting-started/hosting-a-venue-on-openreview/navigating-your-venue-pages.md)).&#x20;

### Decision Stage and Camera Ready Revisions

#### 25. [Submit decisions by Program Chairs](conferences.md#decision-stage)

This is the last step before releasing the decision to the authors. PCs need to submit the final [decisions](https://docs.openreview.net/reference/stages/decision-stage) based on the AC meta reviews and confirmations. Decisions are visible to the PCs only.

For large venues (>2000 submissions) we offer a bulk upload process where the PCs can get the meta reviews values, edit them to meet the acceptance rates and then upload them to the system. The PC console will show the decision stats and decision notes can be edited after they are uploaded.&#x20;

#### 26. [Release decisions to the authors and notify authors](../reference/stages/decision-stage.md)

Once all the decisions are made and uploaded to the system, you may [release](https://docs.openreview.net/reference/stages/post-decision-stage) them to the authors and send email notifications using the [Post Decision stage](../reference/stages/post-decision-stage.md). OpenReview offers a [form](../how-to-guides/communication/how-to-send-decision-notifications-using-the-ui.md) where the PCs can define the email template for each decision.&#x20;

PCs can also (optionally) decide to release the submissions to the public (all the submissions or accepted only) and deanonymize the author names.&#x20;

#### 27. Start camera ready period

The camera ready period starts after the authors are notified about the submission decisions. To begin, open the [Submission Revision Stage](../reference/stages/submission-revision-stage.md). Make sure to enable the setting 'Enable revision for accepted submissions only'.

#### 28. End camera ready period

Authors will no longer be able to edit submissions after the Camera Ready period deadline set in the Submission Revision Stage. After this point, they will need to contact the PCs to allow any revisions after the deadline.

