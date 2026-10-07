# How to remove the Abstract Registration Deadline

If you entered a full submission deadline on your venue request form, your venue has two deadlines. The **Submission** step in your [Workflow Timeline](../../getting-started/hosting-a-venue-on-openreview/navigating-your-venue-pages.md#workflow-timeline) is the abstract registration deadline. The **Full Submission** step then lets authors complete their submissions until the full submission deadline. The deadlines on the venue request form can't be edited after your venue is deployed, so you remove the abstract registration deadline from the Workflow Timeline instead.

Do this before the abstract registration deadline passes:

1. Open the Workflow Timeline for your venue and find the **Full Submission** step.
2. Click **Disable** on the **Full Submission** step. This disables the step and all of its settings.
3. Expand the **Submission** step and click **Edit** next to **Dates**. Set the **Due Date** to your final submission deadline and click **Submit**. The submission form stays open for 30 minutes after this date as a grace period.
4. With two deadlines, the PDF field of the submission form is optional. To make it required, click **Edit** next to **Form Fields** on the **Submission** step and remove `"optional": true` from the `pdf` field. See [Customizing Forms](../../getting-started/customizing-forms.md#where-to-customize-forms) for how to edit the form.

<figure><img src="https://622636955-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FVorH499wd7ipUjYX5etp%2Fuploads%2FAxHZgV5rL9buasDD4jb7%2Fimage.png?alt=media&#x26;token=611d9b6e-ee9a-4375-bb5a-ef5676af2429" alt="The Full Submission step in the Workflow Timeline, with the Disable link next to the step name."><figcaption><p>The Full Submission step and its Disable link, as Program Chairs see it.</p></figcaption></figure>

{% hint style="info" %}
Disable **Full Submission** before you change the **Submission** dates. While **Full Submission** is enabled, you can't set a **Submission** deadline that, with its 30-minute grace period, ends after the **Full Submission** activation date.
{% endhint %}

Once **Full Submission** is disabled, changing the **Submission** deadline also moves the steps that start at the end of the submission period (**Create Submission Change Before Bidding**, **Withdrawal**, **Desk Rejection** and the submission group steps) if they were scheduled earlier than the new deadline.

If the abstract registration deadline has already passed, authors already have a full submission form on each submission. Close those forms before you switch to a single deadline:

1. On the **Full Submission** step, click **Edit** next to **Dates**, set the **Due Date** and **Expiration Date** to the current time, and click **Submit**. This closes the full submission forms on every submission.
2. Click **Disable** on the **Full Submission** step.
3. On the **Submission** step, click **Edit** next to **Dates**, set the **Due Date** to your final deadline, and click **Submit**.

Authors can then update their submissions, including the PDF, through the submission form until the new deadline.
