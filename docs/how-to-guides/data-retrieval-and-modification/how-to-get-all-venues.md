# How to Get All Venues

You can get a list of all of the venues in OpenReview like so:&#x20;

```python
venues = client.get_group(id='venues').members
print(venues)
```
