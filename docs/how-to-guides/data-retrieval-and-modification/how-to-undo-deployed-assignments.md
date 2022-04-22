# How to Undo Deployed Assignments

If you deployed automatic assignments but would like to roll them back, you can do it using the python client.&#x20;

1. If you have not done so, you will need to [install and instantiate the openreview-py client](../../getting-started/using-the-api/installing-and-instantiating-the-python-client.md).&#x20;
2. You will first need to delete all of the assignment edges. You can do this by getting all edges with your venue's assignment invitation and then setting a ddate for each one.

```
edges = list(openreview.tools.iterget_edges(client, invitation = 'Your/Venue/ID/Reviewers/-/Assignment'))
for edge in edges: 
    edge.ddate = 1643083220000
    client.post_edge(edge)
```

3\. Now all of the assignments have been reverted, but before you can deploy a new matching you will need to change the status of the deployed matching configuration from 'Deployed' to 'Complete'. In order to do this, you will first need to find the matching configuration ID. Go to api.openreview.net/notes?invitation=Your/Venue/ID/Reviewers/-/Assignment\_Configuration and find the note with a status of 'Deployed'. Once you have its ID, do the following:

```
note = client.get_note('<note_id>')
note.content['status'] = 'Complete'
client.post_note(note)
```

4\. Now you should have the option to deploy a new matching configuration.
