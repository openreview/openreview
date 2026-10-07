# Introductions to Invitations

See also:

* [Technical Reference for Invitations](../../reference/api-v2/entities/invitation.md)
* How to Guides:
  * [Change Submission Expiration Date](../../how-to-guides/workflow/how-to-change-the-expiration-date-of-the-submission-invitation.md)
  * [Change Submission Readers](../../how-to-guides/workflow/how-to-change-who-can-access-submissions-after-the-deadline.md)
  * [Hide Submission Fields](../../how-to-guides/workflow/how-to-hide-and-unhide-submission-fields-from-reviewers.md)
  * [Customizing Forms](../customizing-forms.md)

In OpenReview, **invitations** define the rules and permissions for creating or editing entities such as [notes](introduction-to-notes.md), [groups](groups.md), and edges. They act as **templates** that control who can perform specific actions, what content is expected, and how submissions are processed.

The vast majority of invitations will be created automatically at the appropriate point of the workflow, and you will interact with invitations via the UI and stages within to change settings.

### Structure of Invitations

See the [API Reference](../../reference/api-v2/entities/invitation.md) for a detailed description of each field of the invitation. The main fields that you will interact with are:

* `id`: A unique identifier (e.g., `ICLR.cc/2025/Conference/-/Area_Chair_Registration`)
* `invitees`: Who is allowed to use the invitation (e.g. Area Chairs)
* `signatures`: Who issued the invitation (typically a venue or program chair group)
* `edit`: Defines the type of object that can be posted (e.g., `note`, `edge`)
* `reply` or `content`: Schema specifying required fields, validation, and structure
* `readers`, `writers`, and `signatures` constraints for resulting entities

### Creating Invitations

The vast majority of invitations will be created automatically at the appropriate point of the workflow, and you will interact with invitations via the UI and stages within to change settings.

In some cases, you may need to create custom invitations to accommodate less commons workflow. In this case, see guidance for those specific stages, or the Custom Stage.

### Editing invitations

1. **Pre-created invitations:** The vast majority of edits to invitations are made from the [Workflow Timeline](../hosting-a-venue-on-openreview/navigating-your-venue-pages.md#workflow-timeline). Examples: [Hiding submission fields](../../how-to-guides/workflow/how-to-hide-and-unhide-submission-fields-from-reviewers.md), [releasing reviews](../../how-to-guides/workflow/how-to-release-reviews-decisions-and-metareviews.md)
2. **Custom Invitations:** For custom invitations that are not configurable in the venue configuration form, you may need to make changes directly to those invitations. In those cases, changes can be made in the Invitation's page, found on your venue's group page: <kbd>http://openreview.net/group/edit?id={venue\_id}</kbd>
3. **Changes to per-paper invitations:** If you need to make programmatic changes to per-paper invitations for many submissions, you may want to make these changes programmatically.

{% hint style="warning" %}
Each time you edit a step, OpenReview recreates its invitation for every submission from the step's settings, so **changes made directly to those invitations are overwritten**.
{% endhint %}

### Deleting invitations

Rather than outright deleting invitations, we use the expiration date. Update the `exp_date` field of the invitation to now or a past time (in ms time), which will stop any new posts from being made with the invitations. Example: [Updating Submission Expiration](../../how-to-guides/workflow/how-to-change-the-expiration-date-of-the-submission-invitation.md). To turn off a whole step of your venue, click **Disable** next to it in the Workflow Timeline.
