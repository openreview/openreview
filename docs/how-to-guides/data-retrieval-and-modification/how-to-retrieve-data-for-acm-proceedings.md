# How to Retrieve Data for ACM Proceedings

Venues uploading their metadata to the ACM e-Rights system will need to follow [specific procedures and requirements](https://www.acm.org/publications/gi-proceedings-current). If you have questions about these requirements, please contact the current ACM point of contact.&#x20;

PCs can use the python client to retrieve the [required submission/author data](https://www.acm.org/binaries/content/assets/publications/taps/papertypes-csvfields-current.pdf), but the CSV or XML file will need to be prepared by PCs.&#x20;

We strongly encourage PCs to communicate with authors as early as possible to fill out the “city” , and “country” data for their institution located in their profiles. PCs can retrieve this data using the openreview-py function: get\_profiles() for one or multiple authors.

```python
profiles = openreview.tools.get_profiles(
    client,
    ids_or_emails=['michael@openreview.net', '~Melisa_bok1']
)
```

To get the affiliation data, check the history within the profile content.

```python
for profile in profiles:
    profile.content['history']
```

The output may look like:

```json
"history": [
    {
    "position": "Researcher",
    "start": 2024,
    "end": null,
    "institution": {
        "domain": "unitn.it",
        "name": "University of Trento",
        "country": "IT",
        "stateProvince": "Trento",
        "city": "Trento",
        "department": "CIMEC"
        }
    }
]
```
