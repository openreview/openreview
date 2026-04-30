---
description: Example workflow in the style of the existing ones
---

# Example Workflow

## 1. Request a Venue

To host a venue on OpenReview—such as a conference, workshop, or symposium—you must first submit a request via the **OpenReview Support Request Form**. This form is used to collect basic information about your venue and define the initial configuration of your review workflow.

#### Steps:

1.  **Go to the Venue Request Page**:

    Visit [OpenReview Request Page](https://dev.openreview.net/group?id=OpenReview.net/Support#tab-venue-configuration-requests).
2.  **Open the Request Form**:

    Select the **"Conference Review Workflow"** form under the _Venue Configuration Requests_ tab.
3. **Fill in the Required Details**
4.  **Submit the Form**:

    Once submitted, the OpenReview team will review your request. They may reach out to confirm details prior to deploying your venue.
5.  **Venue Setup**:

    After approval, a venue group and initial setup will be created in the system. You will gain access to the **Program Chairs Console** for further configuration and management of the workflow.

{% hint style="info" %}
To see an example of a configured UI in an existing venue, use the following link and credentials:\
\
Link: [https://dev.openreview.net/group/edit?id=ICML.cc/2025/Workshop/DataWorld#workflowInvitations](https://dev.openreview.net/group/edit?id=ICML.cc/2025/Workshop/DataWorld#workflowInvitations) email: pc@midl.io \
password: 1234
{% endhint %}

## 2. Complete Workflow Configuration Tasks

After your venue is created, the Program Chairs are responsible for configuring key stages of the peer review workflow. These tasks are listed in the **Workflow Console** under **“Program Chairs Configuration Tasks”** along with their respective deadlines.

Each tasks appears as a clickable item in the configuration panel. Once opened, they present a web form where settings can be customized and saved.

## 3. Review and Configure Workflow Steps

Once the Workflow Configuration Tasks are complete, also review and make any necessary changes to the steps of the workflow. Some stages will run automatically once their activation date passes, so we recommend configuring the stages as early as possible.

## 4. Recruitment

Reviewer recruitment in OpenReview is handled through the **Reviewers Recruitment** workflow stage. This task enables Program Chairs to invite users to participate in the review process—typically as **reviewers**, but potentially also for other roles like **area chairs** or **senior area chairs**, depending on the venue setup.

#### How to Recruit:

1. Go to the **Reviewers Group** in the Workflow Group console
2. Click the **Recruitment Request** button and fill out the Recruitment Form, including users to be invited, and the recruitment template.&#x20;
3.  **Submit changes**

    Once submitted, OpenReview will send personalized email invitations containing links where recipients can accept or decline.

#### Notes:

* Recruitment **can be done at any time**, but it's ideal to complete it well before the review stage begins.
* **Recruitment is independent from assignment matching**. Reviewers can be invited first, and **matching is performed later** once enough reviewers have accepted and submissions are in.
* The Recruitment page can also be used to send **recruitment reminders,** which will send an email to any users that have not already responded to the invitation

## 5. Assignment

Once recruitment is complete and submissions are in, Program Chairs can assign reviewers to papers using the **Create/Deploy Reviewer Assignment** task in the Program Chairs Console.

Assignments are not configured automatically, and must be created by the program chairs.

**How to Assign Reviewers:**

1. Make sure that **Reviewers Conflict** and **Reviewers Affinity Score** have been calculated
2. **Open the Assignment Tool**\
   Click on **“Create Reviewer Assignment”** in the workflow tasks list.\
   This page contains:
   * A link to **create draft assignments**
   * An option to **edit** and configure matching parameters
   * A link to deploy assignments
3. **Create a Draft Match**\
   In the draft assignment interface, you can:
   * Select the pool of eligible reviewers.
   * Choose the matching settings and constraints like maximum and minimum number of papers per reviewer.
4. **Review and Adjust**\
   After generating the draft, review assignments for conflicts of interest, balance, and workload. You can manually reassign reviewers if needed.
5. **Deploy Assignments**\
   Once the assignments are finalized, return to the **Create Reviewer Assignment Deployment** page.
   * Select the correct **Match Name** from the dropdown.
   * Choose a **Deploy Date** (or deploy immediately).
   * Click **Deploy** to make the assignments official.

**Notes:**

* Matching is **done after recruitment** but before the review stage opens.
* Errors during deployment (as seen in the UI) should be resolved by checking that:
  * The draft match exists and is selected.
  * Reviewer groups are correctly populated.
  * Constraints are met

## 6. Monitor Venue

After assignments are made, the rest of the conference workflow will run according to the settings and dates in the conference workflow. Monitor these stages and change them as necessary throughout the rest of the venue workflow.&#x20;



