---
description: How to extract PDFs and zip files associated with submissions.
---

# How to Export all Submission Attachments

1. If you have not done so, you will need to [install and instantiate the openreview-py client](../../getting-started/using-the-api/installing-and-instantiating-the-python-client.md).&#x20;
2. First, get all of the submissions for your venue. If your venue is double blind, do the following:&#x20;

```
notes = list(openreview.tools.iterget_notes(client, invitation = "Your/Venue/ID/-/Blind_Submission", details = 'original'))
```

3\. If your venue is single blind, do the following:&#x20;

```
notes = list(openreview.tools.iterget_notes(client, invitation = "Your/Venue/ID/-/Submission"))
```

4\. Iterate through each submission. For each one, check if it has the attachment you are looking for. In this example, we are exporting pdfs and naming them with the format paper#.pdf.

```
for note in notes:
    if(note.content.get("pdf")):
        f = client.get_attachment(note.id,'pdf')
        with open(f'paper{note.number}.pdf','wb') as op: 
            op.write(f)
```

5\. If you wanted to extract a field called supplementary\_material which authors uploaded as zip files, you could do the following instead:&#x20;

```
for note in notes:
    if(note.content.get("supplementary_material")):
        f = client.get_attachment(note.id,'supplementary_material')
        with open(f'paper{note.number}_supplementary_material.zip','wb') as op: 
            op.write(f)
```
