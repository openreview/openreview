# How to Update a Reviewer's Custom Max Papers

Reviewers have the option to submit a form requesting a custom number of papers to review. The form they submit creates a note with the invitation \<your\_venue\_id>/Reviewers/-/Reduced\_Load. When paper matching setup is run, this note is converted to an edge with the invitation \<your\_venue\_id>/Reviewers/-/Custom\_Max\_Papers. \
To change a reviewers' Custom Max Papers after having run Paper Matching Setup, you can do the following:&#x20;

1. If you have not done so, you will need to [install and instantiate the openreview-py client](../../getting-started/using-the-api/installing-and-instantiating-the-python-client.md).&#x20;
2. Retrieve the custom max papers edge for the reviewer. This edge will have the invitation \<your\_venue\_id>/Reviewers/-/Custom\_Max\_Papers and the reviewer's profile ID as its tail, so you can retrieve it like so:&#x20;

```
edge = client.get_edges(invitation = "<your_venue_id>/Reviewers/-/Custom_Max_Papers", tail = "~User_One1")[0] 
```

The edge has a "weight" field. This represents the custom amount of papers they have agreed to review. To change it, you can set it to the new desired number of papers and repost the note.&#x20;

```
edge.weight = 2
client.post_edge(edge)
```
