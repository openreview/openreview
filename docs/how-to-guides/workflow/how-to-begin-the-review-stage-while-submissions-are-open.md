# How to begin the Review Stage while Submissions are Open

In the [Workflow Timeline](../../getting-started/hosting-a-venue-on-openreview/navigating-your-venue-pages.md#workflow-timeline), the steps that prepare submissions for review are scheduled relative to the submission deadline. To start reviewing while submissions are still open, move the **Activation Date** of each of the following steps to a time before the deadline. For each step, expand it, click **Edit** next to **Dates**, change the **Activation Date** and click **Submit**.

1. **Create Reviewers Submission Group**: creates a Reviewers group for each submission. Once this step is active, a group is also created for each new submission as it comes in.
2. **Create Reviewers Conflict** and **Create Reviewers Affinity Score**: compute conflicts of interest and affinity scores between your reviewers and the submissions that exist when the step runs. Before the activation date, choose the conflict policy with **Edit** next to **Policy** on the conflict step, and the expertise model with **Edit** next to **Model** on the affinity score step.
3. Run the reviewer matching: go to the Assignments page, at `https://openreview.net/assignments?group=<venue_id>/Reviewers`, or use the **Reviewers Paper Assignment** link in your Program Chairs console. Click **New Assignment Configuration**, fill in the form and click **Submit**. Then click **Run Matcher** for that configuration and wait until its status is "Complete".
4. **Create Reviewers Assignment Deployment**: click **Edit** next to **Match** and set **Match Name** to the title of the matching configuration you want to deploy. The configuration must have status "Complete". When the step runs, it deploys the assignments and schedules **Create Submission Change Before Reviewing** to run 30 minutes later. That step gives assigned reviewers access to their submissions.
5. **Official Review**: set the **Activation Date** to when reviewers can start submitting reviews, and set the **Due Date** and **Expiration Date**. Once this step is active, new submissions also get a review form when they're posted.

<figure><img src="https://622636955-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FVorH499wd7ipUjYX5etp%2Fuploads%2F4OPZUPjPinWTIk9GUjZz%2Fimage.png?alt=media&#x26;token=f0685de7-df10-4080-b7d4-39ed4e9a315c" alt="The Create Reviewers Assignment Deployment step expanded in the Workflow Timeline, showing its Dates and Match controls."><figcaption><p>The Create Reviewers Assignment Deployment step, as Program Chairs see it.</p></figcaption></figure>

{% hint style="warning" %}
The **Create Reviewers Assignment Deployment** step deploys assignments only once. If a configuration has already been deployed, the step does nothing when it runs again. PCs can make manual assignments for any submissions that arrive after the first matching is deployed.
{% endhint %}

{% hint style="info" %}
Conflicts and affinity scores only cover the submissions that exist when the steps run. To include later submissions, set the step's **Activation Date** again so it runs once more after the deadline.
{% endhint %}

{% hint style="warning" %}
While submissions are open, authors can still edit them. Each edit sets the submission's readers back to the Program Chairs and its authors, so the committee loses access to it until **Create Submission Change Before Reviewing** runs again. After the submission deadline, set its **Activation Date** to the current time to restore access.
{% endhint %}

{% hint style="warning" %}
If you move the **Submission** deadline (or the **Full Submission** deadline) later, any submission group step scheduled before the new deadline is moved to 30 minutes after it, so the groups are no longer created early. After changing the deadline, check the **Activation Date** of **Create Reviewers Submission Group** and the Area Chair and Senior Area Chair submission group steps, and set them again if needed.
{% endhint %}
