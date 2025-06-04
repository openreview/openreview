# How to test your venue workflow

If you wish to test your venue workflow, please use the dev site, as testing on your live venue page can cause collisions and issues when you run your workflow live. To do so, please go to [dev.openreview.net](https://dev.openreview.net/) and complete the following steps:

1. Create a profile for yourself
2. Submit a venue request
3. Email us at info@openreview.net so that we can deploy the venue for you.

#### Notes about the dev site

* The **dev site uses a separate database** from the live site- this means that no information is shared between the two sites.
* **Emails are disabled** on the dev site, except for **signup confirmation links**. This means that your test emails will not actually be sent, but they will show up in the messages tab of the site.
* If you want to test with **real author names**, you must **create those profiles on the dev site**. Some profiles may already exist from previous testing.
* The Post Decision stage should **only** be run after testing is complete.



### Resetting the Venue

Most stages can be reset by changing the dates and settings to reopen the stage. If you want to reset to the beginning of the review phase:

1. Delete all review [notes](../../reference/api-v1/entities/note/) from the venue
2. Undeploy [assignments](../paper-matching-and-assignment/how-to-undo-deployed-assignments.md) in the assignments browser
3. Re-[assign](../paper-matching-and-assignment/) reviewers as necessary
4. Change the dates to reopen the[ review stage](../../reference/stages/review-stage.md)

