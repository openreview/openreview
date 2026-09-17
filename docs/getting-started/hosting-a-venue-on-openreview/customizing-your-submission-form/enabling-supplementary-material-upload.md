# Enabling Supplementary Material Upload

To add a supplementary material field to the submission form, go to the Workflow Timeline and locate the `Submission` step. Click on "Submission" to expand the configuration for the Submission step and click on Edit next to the Form Fields. You can copy the JSON below and paste it directly to your Content JSON editor:            (make sure to change any details before saving the changes to your submission form)

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
        "description": "All supplementary material must be self-contained and zipped into a single file. Ensure all material is anonymized. The maximum file size is 50MB.",
        "order": 7,
        "readers": [ 
            "ICML.cc/2023/Conference", 
            "ICML.cc/2023/Conference/Submission${4/number}/Authors"
        ]
    }
}
```
{% endcode %}

The field `readers` is optional and it can be used to restrict the readers of the field, if you don't specify the readers then all the readers of the submission will be able to see the supplementary material. Make sure you **use the right group ids** to specify the readers. An placeholder venueid is used in the above example, make sure to change the values for your venue.

This will add a supplementary material field to upload zipped files of size up to 50 MB. You can also enable a Submission Revision Stage to allow a separate deadline for Supplementary Material.
