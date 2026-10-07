# How to release the identities of authors of accepted papers only

Author identities stay hidden when submissions are released, unless you change the **Form Fields** of a release step. The **Accepted Submission Release** step releases submissions with an accepting decision, and the **Rejected Submission Release** step releases the rest. To reveal the authors of accepted papers only, change the **Accepted Submission Release** step and leave the **Rejected Submission Release** step as it is.

1. Go to your venue's [Workflow Timeline](../../getting-started/hosting-a-venue-on-openreview/navigating-your-venue-pages.md#workflow-timeline) and find the **Accepted Submission Release** step. The Workflow Timeline lists it as **Create Accepted Submission Release**.
2. Click **Edit** next to **Form Fields**.
3.  In the **Content JSON** tab, change the `readers` of the `authors` field to:

    ```json
    {
      "authors": {
        "readers": { "const": { "delete": true } }
      }
    }
    ```

    Leave the other fields as they are.
4. Click **Submit**.
5. Click **Edit** next to **Readers** and choose who can read accepted submissions. The authors are revealed to everyone who can read the submission, so select **Public** to show the authors of accepted papers to everyone.
6. Click **Edit** next to **Dates**, set the **Activation Date** to when accepted papers should be released, and click **Submit**.

<figure><img src="https://622636955-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FVorH499wd7ipUjYX5etp%2Fuploads%2FLwuzwXivpf13hTACUa5a%2Fimage.png?alt=media&#x26;token=c8f08a5c-2eb8-4884-94f2-8abd4244b4d1" alt="The Create Accepted Submission Release step expanded in the Workflow Timeline, with the Form Fields editor open on the Content JSON tab showing the default authors entry."><figcaption><p>The default Form Fields of Create Accepted Submission Release, before the authors entry is changed.</p></figcaption></figure>

When the step runs, the authors of accepted papers are shown on the submission and included in its BibTeX. Rejected submissions keep their authors hidden, and their BibTeX lists the author as Anonymous.

{% hint style="info" %}
After the Activation Date has passed, the step runs again each time you edit it. To hide the authors again, set the `readers` of the `authors` field back to a list of readers, such as `["<venue_id>", "<venue_id>/Submission${{4/id}/number}/Authors"]`.
{% endhint %}

For how to make the papers themselves public, see [How to make papers public after decisions are made](how-to-make-papers-public-after-decisions-are-made.md).
