# How to enable the Review Revision Stage

A review revision lets reviewers change a review they already posted after the review deadline has passed, for example after the rebuttal period. The revision form can include existing review fields, so reviewers can update them, and new fields, such as a `final_rating` and a `justification` for changing the rating.

Venues don't have a **Review Revision** step by default. You can create a review revision in one of two ways:

* Ask OpenReview Support by posting a comment on your [venue request form](../../getting-started/hosting-a-venue-on-openreview/navigating-your-venue-pages.md#venue-request-form).
* Create it yourself with the Python client, using the CustomStage. See the [Review Revision example](using-the-customstage.md#review-revision).

Once you create it, it appears in the Workflow Timeline as a **Review Revision** step, where you can change its dates (**Dates**) and its fields (**Form Fields**).

<figure><img src="https://622636955-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FVorH499wd7ipUjYX5etp%2Fuploads%2FV1e269RGExv1B7OkxWI0%2Fimage.png?alt=media&#x26;token=f719c9a5-14f8-4efc-a611-4973ed86f987" alt="A Review Revision step in the Workflow Timeline, created with the CustomStage, showing only its Dates and Form Fields controls."><figcaption><p>A review revision created with the CustomStage, as it appears in the Workflow Timeline.</p></figcaption></figure>

{% hint style="info" %}
The **Rating Field Name** of the **Official Review** step must name a field of the review form itself, so a rating field that only the review revision adds cannot be selected there. To show a new rating field in the Program Chairs console, ask OpenReview Support by posting a comment on your venue request form.
{% endhint %}
