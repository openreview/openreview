# How to manage bidding for Senior Area Chairs

If your venue uses Area Chairs and Senior Area Chairs, your Workflow Timeline will include a bidding step for Senior Area Chairs: **Senior Area Chairs Bid**. By default, Senior Area Chairs are assigned to Area Chairs rather than to submissions, so they bid on the members of your Area Chairs group. To assign them to submissions instead, contact OpenReview Support; this can't be changed from the Workflow Timeline. Their bids can then be used when you compute the Senior Area Chair assignment.

By default, Senior Area Chair bidding opens 3.5 days after the submission deadline (the abstract deadline, if your venue also has a full submission deadline) and is due 7 days after it, and each Senior Area Chair is asked to complete 50 bids. To change these settings:

1. Open your [Workflow Timeline](../../getting-started/hosting-a-venue-on-openreview/navigating-your-venue-pages.md#workflow-timeline) and expand the **Senior Area Chairs Bid** step.
2. Click **Edit** next to **Dates** and set the **Activation Date** (when bidding opens), the **Due Date** and the **Expiration Date** (after which Senior Area Chairs can no longer bid). Click **Submit**.
3. Click **Edit** next to **Settings** to change the **Bid Count** (the number of bids each Senior Area Chair is expected to complete) and the **Labels** Senior Area Chairs can choose from. The default labels are Very High, High, Neutral, Low and Very Low. Click **Submit**.

<figure><img src="https://622636955-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FVorH499wd7ipUjYX5etp%2Fuploads%2FylR2pV5qf6h4objAs2hD%2Fimage.png?alt=media&#x26;token=3054d9f9-582e-47ca-ac47-06a13ecf15e3" alt="The Senior Area Chairs Bid step expanded in the Workflow Timeline, showing its Dates and its Settings with Bid Count 50 and the bid labels."><figcaption><p>The Senior Area Chairs Bid step, as Program Chairs see it.</p></figcaption></figure>

Once bidding is open, Senior Area Chairs bid in the Senior Area Chair Bidding Console, at `https://openreview.net/invitation?id=<venue_id>/Senior_Area_Chairs/-/Bid`.

{% hint style="info" %}
Senior Area Chairs can only bid on users who are members of the Area Chairs group, so [recruit your Area Chairs](../managing-groups/how-to-recruit-and-remind-recruited-reviewers.md) before bidding opens.
{% endhint %}

When you create a Senior Area Chair assignment configuration, the Senior Area Chair bids are included in its scores specification by default, with Very High counting as 1, High as 0.5, Neutral as 0, Low as -0.5 and Very Low as -1.

{% hint style="info" %}
If you do not want to include bidding for Senior Area Chairs in your venue, click **Disable** on the **Senior Area Chairs Bid** step.
{% endhint %}
