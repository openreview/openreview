# Profiles

### Querying profiles

You can retrieve an individual's OpenReview profile object by their name or email:&#x20;

```python
profile = client.get_profile('~Michael_Spector1')
profile = client.get_profile('michael@openreview.net')
```

If you want to query more than one profile at a time, you can use our tools module:

```python
profiles = openreview.tools.get_profiles(
    client,
    ids_or_emails=['michael@openreview.net',
    '~Melisa_bok1'
]
```

If you want to get all the profiles and their publication, you can use the previous call and add the parameter `with_publications=True`

### Finding profile relations

Relations can be extracted in two ways: (1) from the Profile object itself, or (2) from coauthored Notes in the system.

Getting stored relations:

```
>>> profile = client.get_profile('~Michael_Spector1')
>>> profile.content['relations']
[{'name': 'Andrew McCallum',
  'email': ...,
  'relation': ...,
  'start': 2016,
  'end': None},
 {'name': 'Melisa Bok',
  'email': ...,
  'relation': ...,
  'start': 2016,
  'end': None}]
```

Getting coauthorship relations from Notes:

```
>>> profile_notes = client.get_notes(content={'authorids': profile.id})
>>> coauthors = set()
>>> for note in profile_notes:
>>>    coauthors.update(note.content['authorids'])
>>> coauthors.remove(profile.id) # make sure that the list doesn't include the author themselves
>>> print(sorted(list(coauthors)))
```

