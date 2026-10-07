# How to upload paper decisions in bulk

Program Chairs can post decisions one at a time on each submission, or upload them all at once with the **Create Decision Upload** step in the [Workflow Timeline](../../getting-started/hosting-a-venue-on-openreview/navigating-your-venue-pages.md#workflow-timeline).

The upload posts through each submission's decision form, so the **Decision** step must already be open.

1.  Create a CSV file with one decision per line: the paper number, the decision, and an optional comment. Don't add a header row. Each decision must be one of your **Decision Options**.

    ```
    1,Accept,Clear contribution.
    2,Reject,
    3,Accept,
    ```
2. In the Workflow Timeline, expand **Create Decision Upload**, click **Edit** next to **Decision CSV**, upload the file and click **Submit**.
3. Click **Edit** next to **Dates** and set the **Activation Date** to when the decisions should be posted. If that date has already passed, the upload runs shortly after you submit the file.

{% hint style="info" %}
Uploading a file again replaces the decisions of the papers in it. Rows that can't be posted, for example an unknown paper number, are skipped and listed only in the step's **logs**.
{% endhint %}

{% hint style="warning" %}
If the activation date arrives before a file is uploaded, no decisions are posted and a "Decision Upload Failed" comment is posted on your venue request form.
{% endhint %}
