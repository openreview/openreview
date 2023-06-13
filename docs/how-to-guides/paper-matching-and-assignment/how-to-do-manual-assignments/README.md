# How to do manual assignments

{% hint style="info" %}
This top section is only relevant to API 1 venues, scroll down for API 2.
{% endhint %}

**If you selected to use affinity scores, conflicts, bids, or reviewer recommendation scores, you will not be able to use the following options. Instead, follow the guide on** [**how to do automatic assignments**](../how-to-do-automatic-assignments/)**.** &#x20;

### Assigning Reviewers (API 1 venues)

If you did not specify you wanted to use the OpenReview matcher to assign reviewers to papers, you will be able to manually assign them using the PC console.

1. Make sure your submission deadline has passed. [Unless your venue is single blind and public](../../workflow/how-to-begin-the-review-stage-while-submissions-are-open.md), assignments cannot be made until after the submission deadline.
2. Run the review stage by clicking on the [Review Stage](../../../reference/stages/review-stage.md) button on the request form for your venue.
3. Under the 'Paper Status' tab in the PC console, click on 'Show Reviewers' next to the paper you want to assign reviewers to.

To assign reviewers from the reviewer pool, you can choose a reviewer from the dropdown. Here, you can also search reviewers in the reviewer pool by name or email. After finding the reviewer you want to assign, click on the 'Assign' button.

To assign reviewers from outside the reviewer pool, you should type the reviewer's email or OpenReview profile ID (e.g., \~Alan\_Turing1) in the text box and then click on the 'Assign' button. A reviewer does not need to have an OpenReview profile in order to be assigned to a paper.

Note that assigning a reviewer to a paper through the PC console automatically adds that reviewer to the reviewers pool and sends them an email notifying them about their new assignment.

**Area Chairs:** Unfortunately, assigning ACs is not available through the PC console, but manual AC assignments can be done through the Python library: (You can check out the docs for our Python API [here](https://openreview-py.readthedocs.io/en/latest/))

```
client = openreview.Client(baseurl = 'https://api.openreview.net', username = '', password = '')
conference=openreview.helpers.get_conference(client, request_form_id)
conference.set_assignment(number=paper_number, user=user_id, is_area_chair=True)
```

* You will need to use your own OpenReview credentials to initialize the Client object.
* **request\_form\_id** (string) refers to the forum id of the venue request for your venue, (e.g., [https://openreview.net/forum?id=**r1lf10zpw4**](https://openreview.net/faq))
* **paper\_number** (int) is the number of the paper you want to assign an area chair to (you can find this in the 'Paper Status' tab of the PC console)
* **user\_id** (string) is the email address or OpenReview profile ID (e.g., \~Alan\_Turing1) of the user you want to assign

Note that assigning an area chair using python does not send an email to that user. For information on how to contact Area Chairs through the UI, [click here](../../communication/how-to-send-messages-through-the-ui.md). For information about how to contact Area Chairs using python, [click here](../../communication/how-to-send-messages-with-the-python-client.md).&#x20;

### Assigning Reviewers (API 2 venues)

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
