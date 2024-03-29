# How to do manual assignments

**If you selected "Automatic" for Submission Reviewer Assignment, instead follow the guide on** [**how to do automatic assignments**](../how-to-do-automatic-assignments/)**.** &#x20;

{% hint style="info" %}
The "Invite Assignment" button is not currently available for manual assignments.
{% endhint %}

In order to begin assigning reviewers manually for venues using API 2, you must first run the [Paper Matching Setup](../how-to-do-automatic-assignments/how-to-setup-paper-matching-by-calculating-affinity-scores-and-conflicts.md) from the venue request form. You can chose whether or not to calculate COI of affinity scores, but the Paper Matching Setup must be run.

Return to the PC console, in the "Timeline" section you can access the link to the Edge Browser to make manual assignments.

<figure><img src="../../../.gitbook/assets/Screen Shot 2023-06-05 at 2.49.33 PM.png" alt=""><figcaption><p>Link to Edge Browser for reviewer assignments</p></figcaption></figure>

You can follow these instructions on [how to make manual assignments using the edge browser](../how-to-do-automatic-assignments/how-to-make-manual-assignments-with-the-edge-browser-after-deployment.md). Reviewers will be notified when they are assigned to a paper.

The same goes for Area Chairs. Run the Paper Matching Setup and select "area chairs" from the list of roles. Return to the PC console and you will be able to access the assignment link for Area Chairs to the Edge Browser.

<figure><img src="../../../.gitbook/assets/Screen Shot 2023-06-05 at 4.06.37 PM.png" alt=""><figcaption><p>Link to Edge Browser for area chair assignments</p></figcaption></figure>

#### Area Chairs: modifying reviewer assignments

If you would like for Area Chairs to be able to modify reviewer assignments, you can enable this by adding a variable in the Group Content for your venue:&#x20;

1. Go to the main conference group, https://openreview.net/group/edit?id=\<insert\_your\_venue\_id>
2. Scroll down to "**Group Content"** and click 'show content'
3. In the editor, add the variable:&#x20;

```
"enable_reviewers_reassignment": {
    "value": true
  }
```

4. Update Content to save the changes
