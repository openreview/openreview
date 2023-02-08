# Edge

Edges are used to represent matching data, like affinity scores, conflicts, custom quotas, etc. The edge entity contains two main properties: head and tail and optionally a label and weight. Head and tail properties point to other entities in the system like a note, a group and a profile.

## Custom Max Papers

A member of the reviewing committee can specify a maximum number of papers to review. The range of of this value is usually defined by the organizers of the venue. Users can specify this value during the recruitment period or request the value to the organizers. Organizers can post these edges using our Python library. See the example below:

```
client.post_edge(openreview.api.Edge(
    invitation='ICML.cc/2023/Conference/Reviewers/-/Custom_Max_Papers',
    head='ICML.cc/2023/Conference/Reviewers',
    tail='~Reviewer_ICMLOne1',
    signatures=['ICML.cc/2023/Conference/Program_Chairs'],
    weight=2
))
```

Please make sure the tail value is the profile id of the user. To get the profile id you can use the following call:

```
profile = openreview.tools.get_profile('reviewer@icml.cc')
profile.id
```

Once the edge is posted, it will appear in the reviewers console as the following image:

<figure><img src="../../../.gitbook/assets/Screen Shot 2023-01-31 at 11.14.16 AM.png" alt=""><figcaption></figcaption></figure>

## Bids

Bids are also represented as edges, where the head is the paper id and the tail is the profile id of the user that is doing the bid. As we said before, bids have an optional property called "label" and in this case is being used to represent the type of bid, for example: "Very High", "High", "Neutral", "Low" and "Very Low".

Users, members of the official committee, usually use the "Bidding Console" to create and edit these bids but the venue organizers can edit them.&#x20;

#### Edit bids:

Below is an example to edit a bid, every time you update a bid you need to set the signatures. The signatures must be a group id where you have signatory permissions. The bid invitation specifies the signature must be either a profile id or the venue id, in the case below is "ICML.cc/2023/Conference":

```
edge = client.get_edge(bid_id)
edge.label = 'Very High'
edge.signatures = ["ICML.cc/2023/Conference"]
edge.nonreaders = None
client.post_edge(edge)
```

#### Remove bids:

To remove bids, you would need to a the ddate value using a timestamp in milliseconds. This edit will perfrom a soft delete meaning this bid can be restored.

<pre><code>edge = client.get_edge(bid_id)
<strong>edge.ddate = 1664467200000
</strong>edge.signatures = ["ICML.cc/2023/Conference"]
edge.nonreaders = None
client.post_edge(edge)
</code></pre>

#### Restore bids:

To restore bids, you would need to get the deleted bid and remove the ddate value. The way API 2 handles the deletion of values in an object is using "{ delete: true }". Be aware the invitation must specify that field with the parameter "deletable: true" otherwise this action can bit be performed.

```
edge = client.get_edge(bid_id)
edge.ddate = { 'delete': True }
edge.signatures = ["ICML.cc/2023/Conference"]
edge.nonreaders = None
client.post_edge(edge)
```

