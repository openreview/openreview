# How to customize your venue homepage

You can customize your venue homepage using Markdown. All Markdown should be validated to ensure that it will not break the layout of the page: [https://stackedit.io/](https://stackedit.io/)

### Group Content&#x20;

Go to your homepage https://openreview.net/group?id=Your/Venue/ID

1. Click on "Edit Group" button above the title of the venue.
2. Scroll down to "Group Content", click to open the edit box and find the field "instructions".
3. In the  "value",  you can include your instructions in markdown or html as a string.

<figure><img src="../../.gitbook/assets/Screenshot 2025-03-12 at 2.12.56 PM.png" alt=""><figcaption><p>View of the Group Content edit box, highlighting the instructions field</p></figcaption></figure>

Markdown example, to customize:

```
"# Important Dates\n
- Submission Deadline: Sept 11, 2021 11:59pm AOE\n
- Author Response Period: Oct 11, 2021 - Oct 18, 2021\n
- Decision Notification: Nov 1, 2021\n
- Camera Ready: Nov 15, 2021\n\n
# Submission Format\n
- Proceedings track: 8 pages. Findings (extended abstract) track: 4 pages. Excluding references and appendices.\n
- Submissions must be anonymous for the double-blind review process\n
- Latex template: [Proceedings](https://www.overleaf.com/latex/templates/proceedings-template/trtrvjxchjdw) or [Findings](https://www.overleaf.com/latex/templates/ml4h-2024-findings-template/vkvrxcgdkcmn)"
```

which will be displayed as:

![homepage after modifying the instructions field](<../../.gitbook/assets/Screenshot 2025-03-11 at 4.29.39 PM.png>)
