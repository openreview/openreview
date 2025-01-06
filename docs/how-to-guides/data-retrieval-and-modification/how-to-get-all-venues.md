# How to Get All Venues

You can get a list of all of the venues in OpenReview like so:&#x20;

```python
venues = client.get_group(id='venues').members
print(venues)
```

This will give you the list of venue\_ids for all venues in OpenReview.&#x20;

To get additional information about a given API 2 venue, you can use the `venue_id` to get the specific venue group such as in the following:&#x20;

```python
venue_group = live_client_v2.get_group(venue_id)
```

Then you can query the properties of this group to get additional information about the conference. For example, to get the submission id used by a conference, you could use the following method, where `content` is a dictionary of information about the venue, the key is the name of the property, and the value is a JSON object with field such as `'value'`.&#x20;

```python
venue_group.content['submission_id']['value']
```

Keys ending in `_id` give the id's used by a conference for specific invitations or groups. For example, the above may be useful if you want to get the submissions for a conference, but don't know the invitation id for submissions.

To see all possible keys contained in a venue group's `content` object, you can use the following:

```python
venue_group.content.keys()
```
