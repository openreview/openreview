# Mental Model on Blind Submissions and Revisions

{% hint style="info" %}
This is only valid for API V1.

API V2 allows to hide fields in the same Note. Therefore, blind submissions are unnecessary because author related fields can have restricted readership.
{% endhint %}

OpenReview anonymizes author information for double blind conferences using the Blind Submission invitation. This information may later be revealed depending on the policies of the conference.

All the submissions in a Conference are represented by Notes in OpenReview. Therefore, a Note will contain all the Submission information. In order to anonymize a Note, another Note pointing to the original Note is created, as can be seen in the diagram below. This new Note is an exact copy of the original Note except that it hides the authors’ identities.

The References pointing to the Notes through the referent in this example are revisions done to the submission. Revision1, Revision2 and Revision3 also contain the authors’ identities. In order to hide this information, a blind Reference is also created which masks the sensitive information of the References. All of this is done so that more users are allowed to see paper submissions without knowing the names of the authors.

![](<../.gitbook/assets/Screen Shot 2022-05-12 at 3.47.10 PM.png>)
