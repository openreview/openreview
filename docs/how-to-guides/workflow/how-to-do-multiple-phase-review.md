# How to do multiple phase review

Venues may want to have some portion of submissions be revised and allow for another round of review. For venues where the same reviewers will review the revisions to the submissions, the workflow can be configured by a Program or Workflow Chair:&#x20;



1. After the first round of review, post [Decisions](../../reference/stages/decision-stage.md) for all submissions _not_ receiving a second round of review. These can be Accept decisions, Reject decisions, or some combination of the two.&#x20;
2. Run the [Post Decision ](../../reference/stages/post-decision-stage.md)Stage which will update venue id of the the papers with decisions.
3. Set up the [Submission Revision](../../reference/stages/submission-revision-stage.md) Stage and [Review Revision](how-to-enable-the-review-revision-stage.md) Stage. Ensure that the activation date is in the future. Once the stage is created, edit the Group Content of each invitation include the following `source` variable. This ensures that the revision will only be active for submissions that do not yet have decisions.

```
{
  "source": {
    "value": {
      "value": {
        "venueid": [
          "YOUR/VENUE/ID/Submission"
        ]
      }
    }
  }
}
```

4. (Optional) Send a message through the UI by selecting the papers with No Decision and sending a message to authors, or [programmatically through the Python Client](../communication/how-to-send-messages-with-the-python-client.md)
5. (Optional) You can also add the same `source` variable to the Official Comment Stage to allow for discussion only on the submissions under revision.
6. Once the re-reviews are complete, add the Decisions to the revised papers and run the Post Decision Stage once again.



For significant variations on this workflow, such as new reviewer assignment, more customization of the workflow may be necessary. [Contact Support ](https://openreview.net/contact) with the details of your workflow for additional information.
