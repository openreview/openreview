# How to Undo Deployed Assignments

If you deployed automatic assignments but would like to roll them back, you can do it using the python client.&#x20;

{% hint style="info" %}
Warning: This will delete all assignment edges, including manual modifications made by ACs or SACs. If you want to keep these modifications, make sure to add them to your new matching's proposed assignments before proceeding.
{% endhint %}

1. If you have not done so, you will need to [install and instantiate the openreview-py client](../../getting-started/using-the-api/installing-and-instantiating-the-python-client.md).&#x20;
2.  You will first need to delete all of the assignment edges. You can do this by getting all edges with your venue's assignment invitation and then setting a `ddate` for each one. Depending on the size of your venue, this may take several minutes.

    ```python
    import datetime

    grouped_edges = client.get_grouped_edges(invitation='Your/Venue/ID/Reviewers/-/Assignment', groupby='head')

    for group in grouped_edges:
        for edge in group['values']:
            edge = openreview.api.Edge.from_json(edge)
            edge.ddate = openreview.tools.datetime_millis(datetime.datetime.utcnow())
            client.post_edge(edge)
    ```
3.  Now all of the assignments have been reverted, but before you can deploy a new matching you will need to change the status of the deployed matching configuration from 'Deployed' to 'Complete'. In order to do this, you will first need to find the matching configuration ID. \
    \
    First, find the assignment configuration note with a status of 'Deployed':\
    \
    client.post\_note\_edit(invitation='Your/Venue/ID/-/Edit',

    ```python
    notes = client.get_notes(invitation='Your/Venue/ID/Reviewers/-/Assignment_Configuration')
    for note in notes:
        if note.content['status']['value'] == 'Deployed':
            configuration_note = note
            break
    ```
4.  Once you have this note, do the following:

    ```python
    client.post_note_edit(invitation='Your/Venue/ID/-/Edit',
    signatures=['Your/Venue/ID'],
    note=openreview.api.Note(
      id=configuration_note.id,
      content={
         'status': { 'value': 'Complete' }
      }
    ))
    ```


5. Now you should have the option to deploy a new matching configuration.

