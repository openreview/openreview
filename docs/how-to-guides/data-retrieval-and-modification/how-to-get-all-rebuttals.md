# How to get all Rebuttals

For some venues, rebuttals are made by a specific invitation. PCs select the value for this in the venue configuration form in the 'Number Of Rebuttals' field.&#x20;

### Using the Rebuttal invitation

If the venue used a Rebuttal invitation (most common scenario), then you can use the following steps to collect all rebuttals for the venue.

#### 1. [Install and instantiate the Python Client](../../getting-started/using-the-api/installing-and-instantiating-the-python-client.md)

#### 2. Get all submissions (with direct replies)

<pre class="language-python"><code class="lang-python"><strong>venue_id = VENUE_ID
</strong><strong>venue_group_settings = client.get_group(venue_id).content
</strong>submission_invitation = venue_group_settings['submission_id']['value']
submissions = client.get_all_notes(
    invitation=submission_invitation,
    details='directReplies'
)
</code></pre>

See also: [How do I find a venue id?](../../getting-started/frequently-asked-questions/how-do-i-find-a-venue-id.md)

#### 3. Filter replies for Rebuttals

```python
rebuttals = []
for s in submissions:
    for r in s.details['directReplies']:
        if any(invitation.endswith('Rebuttal') for invitation in r['invitations']):
            rebuttals.append(r)
```

### Using Official Comments to get rebuttals

An alternative method, which for example can be used for venues that do not have a specific rebuttal invitation, you can get official comments for the venue, then filter the comments to those signed by the authors. Please note that this may include some non-rebuttal comments. See: [How to get all official comments](how-to-get-all-official-comments.md) for further description.
