# How to customize emails sent through OpenReview

Emails can be sent to users of your venue programmatically using the API or from certain pages like the Program Chair console. They can be personalized to include the recipient's name or other information using email template tags.

There are two types of email template tags: Tags that are handled on the backend by the OpenReview API, and tags that are replaced on the frontend (in the browser).&#x20;

Backend tags can be used anywhere, including when sending messages directly using the API or via the openreview-py library. Frontend tags can only be used on specific pages, such as the Area Chair console and the Program Chair console. A list of all available tags is below:

Backend tags: `{{fullname}}`, `{{firstname}}`

Frontend tags: `[[SUBMIT_REVIEW_LINK]]`

If you want to include further customizations, including links to papers or reviews, you can [message users with the python client.](how-to-send-messages-with-the-python-client.md)
