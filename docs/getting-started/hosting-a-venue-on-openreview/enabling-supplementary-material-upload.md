# Enabling Supplementary Material Upload

You can add supplementary material to the submission form by clicking on the 'Revision' button and adding the following JSON under Additional Submission Options:

{% code title="API 2" %}
```
{
    "supplementary_material": {
        "value": {
            "param": {
                "type": "file",
                "extensions": ["zip", "pdf"],
                "maxSize": 50,
                "optional": true,
                "deletable": true
            }
        },
        "description": "All supplementary material must be self-contained and zipped into a single file. Note that supplementary material will be visible to reviewers and the public throughout and after the review period, and ensure all material is anonymized. The maximum file size is 100MB.",
        "order": 7,
        "readers": [ 
            "ICML.cc/2023/Conference", 
            "ICML.cc/2023/Conference/Submission${4/number}/Senior_Area_Chairs", 
            "ICML.cc/2023/Conference/Submission${4/number}/Area_Chairs", 
            "ICML.cc/2023/Conference/Submission${4/number}/Authors"
        ]
    }
}
```
{% endcode %}

The field `readers` is optional and it can be used to restrict the readers of the field, if you don't specify the readers then all the readers of the submission will be able to see the supplementary material. Make sure you use the right group ids to specify the readers.

This will add a supplementary material field to upload zipped files of size up to 50 MB. You can also enable a Submission Revision Stage to allow a separate deadline for Supplementary Material.&#x20;

If your venue is using the API 1 (api\_version = "1") then you should use the following JSON example:

{% code title="API 1" %}
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
{% endcode %}
