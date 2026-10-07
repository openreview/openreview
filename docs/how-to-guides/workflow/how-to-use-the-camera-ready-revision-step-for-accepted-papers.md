# How to use the Camera Ready Revision step for accepted papers

Venues created with the Conference Review Workflow include a **Camera Ready Revision** step by default. When it activates, OpenReview creates a camera-ready revision form for each submission with an accepted decision, and only that submission's authors can use it.

To make changes to the default settings:

1. Go to your venue's [Workflow Timeline](../../getting-started/hosting-a-venue-on-openreview/navigating-your-venue-pages.md#workflow-timeline) and find the **Camera Ready Revision** step.
2. Make sure the decision options that mean acceptance are set correctly. In the **Decision** step, click **Edit** next to **Decision Options** and check the **Accept Decision Options**. The default decision options are "Accept" and "Reject", and "Accept" is the only accept option. A submission is treated as accepted only if its decision is one of the accept options.
3. In the **Camera Ready Revision** step, click **Edit** next to **Dates** and set the dates:
   * **Activation Date**: when the revision form opens to authors of accepted papers.
   * **Due Date**: when camera-ready revisions are due.
   * **Expiration Date**: when the revision form closes.
4. Click **Submit**.
5. (Optional) To change the fields on the camera-ready form, click **Edit** next to **Form Fields**. By default, the form contains the fields from the default submission form, without the `email_sharing` and `data_release` fields. See [Customizing Forms](../../getting-started/customizing-forms.md) for how to edit form fields.

<figure><img src="https://622636955-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FVorH499wd7ipUjYX5etp%2Fuploads%2FO1xA9Nuwx45hSybamuxN%2Fimage.png?alt=media&#x26;token=7b5cd6b2-c44f-4989-8b94-1779b749efed" alt="The Camera Ready Revision step expanded in the Workflow Timeline, showing its Dates and Form Fields controls."><figcaption><p>The Camera Ready Revision step, as Program Chairs see it.</p></figcaption></figure>

{% hint style="info" %}
Post the decisions in the **Decision** step before the camera-ready **Activation Date**. The camera-ready form is created per submission, only for submissions that already have an accepted decision when the step runs. The step runs on the **Activation Date** and again each time you edit it, so if a decision is posted or changed later, click **Edit** next to **Dates** and submit the form again.
{% endhint %}

When an author submits a camera-ready revision, the changes are applied directly to the submission, so the submission always shows the latest version. To retrieve camera-ready submissions programmatically, see [How to get all notes for submissions, reviews, rebuttals, etc.](../data-retrieval-and-modification/how-to-get-all-notes-for-submissions-reviews-rebuttals-etc.md)
