# Enabling Supplementary Material Upload

You can add supplementary material to the submission form by clicking on the 'Revision' button and adding the following JSON under Additional Submission Options:

```
{
  "supplementary_material": {
    "description": "Supplementary material (e.g. code or video). All supplementary material must be self-contained and zipped into a single file.",
    "order": 10,
    "value-file": {
        "fileTypes": [
            "zip"
        ],
        "size": 50
    },
    "required": false
  }
}
```

This will add a supplementary material field to upload zipped files of size up to 50 MB. You can also enable a Submission Revision Stage to allow a separate deadline for Supplementary Material.&#x20;
