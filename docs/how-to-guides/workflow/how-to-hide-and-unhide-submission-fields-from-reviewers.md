# How to Hide and Unhide Submission Fields from Reviewers

Two steps in your venue's [Workflow Timeline](../../getting-started/hosting-a-venue-on-openreview/navigating-your-venue-pages.md#workflow-timeline) decide which submission fields reviewers can see:

* **Submission Change Before Bidding** runs 30 minutes after the submission deadline. It gives all Reviewers access to the submissions so they can bid. By default it hides the author identities and the PDF.
* **Submission Change Before Reviewing** gives assigned Reviewers access to the submissions they review. By default it hides the author identities, and the PDF becomes visible again.

A hidden field is visible only to the readers you list for it. By default, these are the venue group, which includes the Program Chairs, and the submission's authors.

1. Go to the Workflow Timeline and find the **Submission Change Before Reviewing** step. The Workflow Timeline lists it as **Create Submission Change Before Reviewing**.
2. Click **Edit** next to **Restrict Field Visibility**. The **Content Readers** editor shows the current settings as JSON. The `authors` entry is already there.
3.  Add an entry for each field you want to hide, using the field name from your submission form. Copy the `readers` list from the `authors` entry. For example, to hide a field named `financial_aid`:

    ```json
    {
      "authors": {
        "readers": ["<venue_id>", "<venue_id>/Submission${{4/id}/number}/Authors"]
      },
      "financial_aid": {
        "readers": ["<venue_id>", "<venue_id>/Submission${{4/id}/number}/Authors"]
      }
    }
    ```

    Keep the other entries that are already in the editor, and set the `pdf` entry to `"pdf": { "readers": { "const": { "delete": true } } }` so assigned reviewers can see the PDF. To keep the PDF hidden from them instead, give the `pdf` entry the same `readers` list.
4. Click **Submit**.
5. To hide the field while Reviewers bid as well, repeat steps 2 to 4 in the **Submission Change Before Bidding** step.

<figure><img src="https://622636955-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FVorH499wd7ipUjYX5etp%2Fuploads%2FYl0TUmKhODsYDnt97OkP%2Fimage.png?alt=media&#x26;token=dd7762f5-8d58-4ce8-9434-47a5b3f140bd" alt="The Create Submission Change Before Reviewing step expanded in the Workflow Timeline, with the Restrict Field Visibility editor open showing the default Content Readers JSON."><figcaption><p>The default Restrict Field Visibility settings of Create Submission Change Before Reviewing, as Program Chairs see them.</p></figcaption></figure>

{% hint style="info" %}
Each step applies its settings on its **Activation Date**. After that date has passed, the step runs again each time you edit it, so your changes reach existing submissions right away. When you deploy assignments, the Activation Date of **Submission Change Before Reviewing** moves to 30 minutes after the deployment. To run the step now, click **Edit** next to **Dates** and set the **Activation Date** to the current time.
{% endhint %}

{% hint style="warning" %}
The default settings of **Submission Change Before Reviewing** make the PDF visible to assigned Reviewers again with this `pdf` entry:

```json
"pdf": { "readers": { "param": { "const": { "delete": true } } } }
```

If you submit the editor with this entry unchanged, the `pdf` entry is dropped and the PDF stays hidden from assigned Reviewers. Replace it with the entry below before you click **Submit**.
{% endhint %}

## Make a hidden field visible again

To make a field visible to everyone who can read the submission again, set its `readers` to `{ "const": { "delete": true } }` in the **Content Readers** editor. For example, to show the PDF:

```json
"pdf": { "readers": { "const": { "delete": true } } }
```

Keep the other entries, click **Submit**, and the step removes the field's restriction from existing submissions the next time it runs.
