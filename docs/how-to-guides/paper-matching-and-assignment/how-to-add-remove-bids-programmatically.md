# How to add/remove bids programmatically

Bids are represented by Edges where the `head` is the Note `id` of the submission and the `tail` is the Profile `id` of the user that is doing the bid. Bids have an optional property `label` and in this case it is being used to represent the type of bid, for example: "Very High", "High", "Neutral", "Low" and "Very Low".

Members of the official committee usually use the "Bidding Console" to create and edit these bids. However, bids can also be updated programmatically.

#### Editing bids:

Every time you update a bid you need to set the signatures. The signatures must be a group id where you have signatory permissions. The bid Invitation specifies that the signature must be either a profile id or the venue id. In the example below, the signature is "ICML.cc/2023/Conference":

```python
edge = client.get_edge(bid_id)
edge.label = 'Very High'
edge.signatures = ["ICML.cc/2023/Conference"]
edge.nonreaders = None
client.post_edge(edge)
```

#### Removing bids:

To remove bids, you need to a the `ddate` value using a timestamp in milliseconds. This change will perform a soft delete which means that this bid can be restored.

<pre class="language-python"><code class="lang-python">edge = client.get_edge(bid_id)
<strong>edge.ddate = 1664467200000
</strong>edge.signatures = ["ICML.cc/2023/Conference"]
edge.nonreaders = None
client.post_edge(edge)
</code></pre>

#### Restoring bids:

To restore bids, you need to get the deleted bid and remove the `ddate` value. This is achieved by passing an object `{ delete: true }`. Be aware that the invitation must allow a this operation by passing an object with "`deletable: true`", otherwise, this action cannot be performed.

```python
edge = client.get_edge(bid_id)
edge.ddate = { 'delete': True }
edge.signatures = ["ICML.cc/2023/Conference"]
edge.nonreaders = None
client.post_edge(edge)
```
