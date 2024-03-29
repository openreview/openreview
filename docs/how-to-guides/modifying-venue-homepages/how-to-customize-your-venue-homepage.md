# How to customize your venue homepage

There are two ways to customize your venue homepage using Markdown. All Markdown should be validated to ensure that it will not break the layout of the page: [https://stackedit.io/](https://stackedit.io/)

### Group Editor&#x20;

Go to your homepage https://openreview.net/group?id=Your/Venue/ID

1. Click on "Edit Group" button above the title of the venue.
2. Scroll down to "Group UI Code" and click on the "Show Code Editor"
3. Scroll down to the VenueHomepage component, and within the header edit the "instructions" field.

<figure><img src="../../.gitbook/assets/Screen Shot 2023-07-26 at 2.46.05 PM.png" alt="" width="375"><figcaption><p>View of the Group UI code highlighting the instructions field</p></figcaption></figure>

### Homepage Override field&#x20;

On the request form for your venue, click on the ‘Revision’ button and modify the Homepage Override field, which expects valid JSON.

The instruction field of the JSON accepts Markdown, allowing the formatting of the instructions section to be fully customized.&#x20;

Example:

<figure><img src="../../.gitbook/assets/Screen Shot 2023-07-26 at 4.21.36 PM.png" alt=""><figcaption><p>Homepage override options</p></figcaption></figure>

which will be displayed as:

![homepage after modifying the Homepage Override field](https://openreview.net/images/faq-homepage-override.png)
