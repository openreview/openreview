# How to Make Submissions Available Before the Submission Deadline

Submissions are released to your committee by the **Create Submission Change Before Bidding** step in your [Workflow Timeline](../../getting-started/hosting-a-venue-on-openreview/navigating-your-venue-pages.md#workflow-timeline). By default this step runs automatically 30 minutes after the submission deadline, and it gives all Senior Area Chairs, Area Chairs and Reviewers access to all submissions, with author identities and PDFs hidden. To release submissions earlier, move the step's activation date forward.

1. Open the Workflow Timeline for your venue and expand the **Create Submission Change Before Bidding** step.
2. Click **Edit** next to **Readers** to choose who can see the submissions. The options are **Program Chairs**, **All Senior Area Chairs**, **All Area Chairs**, **All Reviewers**, **Submission Authors** and **Public** (the Senior Area Chair and Area Chair options only appear if your venue has those roles).
   * To make submissions public, select **Public** on its own. It can't be combined with other options.
   * Otherwise, **Program Chairs** and **Submission Authors** must both be selected.
3. Click **Edit** next to **Restrict Field Visibility** if you want to change which submission fields stay hidden. For more on this, see [How to Hide Submission Fields from Reviewers](how-to-hide-and-unhide-submission-fields-from-reviewers.md).
4. Click **Edit** next to **Dates** and set the **Activation Date** to the time you want submissions released. Click **Submit**.

<figure><img src="https://622636955-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FVorH499wd7ipUjYX5etp%2Fuploads%2Fs1Pa1Pvq1jnTb1cQ908N%2Fimage.png?alt=media&#x26;token=b276b7ce-bb9c-4aca-b62e-5fac4f365900" alt="The Create Submission Change Before Bidding step expanded in the Workflow Timeline, with the Readers editor open and the Public option shown in the reader dropdown."><figcaption><p>The Readers editor of Create Submission Change Before Bidding, as Program Chairs see it.</p></figcaption></figure>

When the activation date arrives, the step updates every active submission with the readers you chose. If you selected **Public**, each submission also gets a publication date and a BibTeX entry.

{% hint style="info" %}
The step only updates submissions that exist when it runs. Submissions posted afterwards are not released until the step runs again. While submissions are open, authors can still edit them. Each edit sets the submission's readers back to the Program Chairs and its authors, so the committee loses access to it until the step runs again. To include new submissions and restore access to edited ones, set the **Activation Date** again to a time after the submission deadline, so the step runs once more when all submissions are in.
{% endhint %}

{% hint style="warning" %}
If you later change the dates of the **Submission** step (or the **Full Submission** step, if your venue has two deadlines), a **Create Submission Change Before Bidding** activation date that is earlier than the end of the new submission period (the deadline plus the 30-minute grace period) is moved to that time.
{% endhint %}
