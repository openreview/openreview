# Overview

### 1. Introduction

This guide is for venue organizers using the new workflow configuration UI in OpenReview. This guide will walk through an overview of how to best interact with the UI.



{% hint style="info" %}
To see an example of a configured UI in an existing venue, use the following link and credentials:\
\
Link: [https://dev.openreview.net/group/edit?id=ICML.cc/2025/Workshop/DataWorld#workflowInvitations](https://dev.openreview.net/group/edit?id=ICML.cc/2025/Workshop/DataWorld#workflowInvitations) email: pc@midl.io \
password: 1234
{% endhint %}



### **2. Accessing the Workflow Console**

Once your venue is created and you're assigned as a Program Chair, navigate to the console by:\
&#x20;`Program Chairs Console → Workflow Setup Timeline`

This brings you to the **Workflow Setup Timeline** tab, where you can:

* View user groups (reviewers, authors, chairs)
* Configure review stages (bidding, assignments, reviews, decisions)
* Set email notifications and visibility options
* Automate release of submissions, reviews, and decisions

### **3. Understanding the UI Structure**

#### **3.1 Workflow Groups**

This section let's you view the members of major group in the venue.

* **Program Chairs** – oversee all configurations and decisions
* **Reviewers** – assigned to review submissions (also used for recruiting reviewers)
* **Authors** – those submitting papers
* **Automated Administrator** – used for auto-execution of stages like conflict detection

#### **3.2 Program Chair Configuration Tasks**

This section includes tasks to help guide the Program Chair through configuring major steps of the venue. Clicking on each task will take you to the relevant section of the workflow.&#x20;

#### **3.3 Workflow Configurations**

This section contains a **chronological** list of stages in the submission and review pipeline. Each row shows:

* A **stage title** (e.g., “Create Reviewers Conflict”)
* An **invitation** label showing who can access it
* The **Enable / Disable** toggle
* Execution status (e.g., _Executed_, _Scheduled_, _Expired_)
* A link to view or edit the configuration form
* A color - Green means the stage is complete, Blue means stage is in progress, Grey means not started yet

### **4. How to Edit a Workflow Stage**

You can customize each stage via its configuration form.

1. Locate the stage in the **Workflow Configurations** list.
2. If not yet active, click **Enable**.
3. Click the **stage title** (e.g., “**Create Author Decision Notification**”).
4. In the configuration form you use the Edit button to change settings such as:&#x20;
   * Set or adjust start and end dates
   * Customize email messages
   * Choose which users have access
   * Define visibility and data behavior
5. Click **Submit** or **Save** to apply changes. This will also re-order the stages, if the dates were changed.

#### **Notes:**

* Read the description of each invitation and field to understand how it works in the workflow.
* Re-execution of stages like “Create Reviewers Affinity Score” may overwrite existing data.
* When editing forms, use the '**Preview**' tab to see what the form will look like.&#x20;

### **5. Key Workflow Stages Overview**

#### **Submissions**

* **Submission**: Opens the form for authors to submit papers. This invitation is time-bound and should end before reviewer assignment begins.
* **Withdrawal**: Enables authors to withdraw papers.
* **Desk Rejection**: Allows chairs to reject papers prior to review.

#### **Reviewer Assignment**&#x20;

* **Reviewers Bid**: Enables reviewers to bid for reviewing specific papers.
* **Create Reviewers Conflict**: Detects institutional or co-authorship conflicts.
* **Create Reviewers Affinity Score**: Computes expertise match scores using OpenReview's models.
* **Create Reviewers Submission Group**: Creates a reviewer group for each paper. These groups record which reviewers are assigned to a submission, and need to be created before assignment begins.&#x20;
* **Create Reviewer Assignment Deployment**: Finalizes reviewer-paper assignments based on prior bidding, affinity, or manual assignment.

#### **Review Stage**

* **Create Author Reviews Notification**: Defines and sends notification emails to authors when reviews are ready.
* Replies to Submissions
  * **Official Review**: Launches the review form reviewers must fill out.
  * **Official Comment:** Allows for comments to be made on the submission
  * **Author Rebuttal**: (If enabled) lets authors respond to reviews.
* Stats:&#x20;
  * **Create Reviewers Review Assignment Count:** Count number of assigned reviews for each reviewer
  * **Create Reviewers Review Days Late:** Compute the days late for reviewers
  * **Create Reviewers Review Count:** Compute the number of reviews per reviewer

#### **Decision Stage**

* **Create Decision Upload**: Upload final decisions in batch (e.g., from a CSV).
* **Create Author Decision Notification**: Customize notification email content for decisions.
* **Decision Release**: Specifies the release time and visibility settings for decisions.

#### **Final Submissions**

* **Camera Ready Revision**: Opens the form for accepted papers to be revised for camera-ready versions.
* **Create Submission Release**: Controls which submissions are made public and under what license.

### **6. Customization Tips**

* Review and customize all stages prior to starting your venue - stages can now be scheduled to release ahead of time.
* Automated stages (e.g., affinity score, conflict detection) are executed by the **Automated Administrator** group.

