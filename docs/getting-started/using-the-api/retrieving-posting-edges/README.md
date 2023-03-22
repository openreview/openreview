# Edges

Bids, assignments, affinity scores, conflicts, etc. are saved as _Edges_ in OpenReview.

{% hint style="info" %}
You can read more about Edges in the reference section depending on the API version: [Edges V1](../../../reference/api-v1/entities/edge/), [Edges V2](../../../reference/api-v2/entities/edge/)
{% endhint %}

Simply speaking, _Edges_ are links between two OpenReview entities (_Notes_, _Groups_, _Profiles_, etc.).

Besides the fields that define user permissions, an _Edge_ would usually contain these fields: _head_, _tail_, _weight_, _label_.

For example, a OpenReview affinity score edge for a paper-reviewer pair may have the reviewer’s _Profile_ id set in the _edge.head_ field, paper id set in the _edge.tail_ field, and OpenReview affinity score set in the _edge.weight_ field.

All _Edges_ respond to some OpenReview _Invitation_, which specifies the possible content and permissions that an _Edge_ is required to contain.

Because the amount of _Edges_ can be quite large, depending on the query used, you need to pass at least one of the following parameters: `id`, `head`, `tail`.

{% hint style="info" %}
For more information on the available parameters, check the corresponding OpenAPI reference: [Edges V1](../../../reference/api-v1/openapi-definition.md#edges), [Edges V2](../../../reference/api-v2/openapi-definition.md#edges).

Note that the `groupby` parameter is only available in the method `get_grouped_edges` described below.
{% endhint %}

Consider the following example which gets the first 10 _Edges_ representing the “Personal” conflicts in ICLR 2020:

```python
conflict_edges = client.get_edges(
    invitation='ICLR.cc/2020/Conference/-/Conflict',
    label='Personal',
    tail='~Test_User1',
    limit=10
)
```

Note that since conflict data is sensitive, you may not have permissions to access conflict edges mentioned in the above example.

By default, _get\_edges_ will return up to the first 1000 corresponding _Edges_ (_limit=1000_). To retrieve _Edges_ beyond the first 1000, you can adjust the _offset_ parameter, or use the function _get\_all\_edges_ which returns a list of all corresponding _Edges_:

```python
conflict_edges_iterator = openreview.tools.iterget_edges(
    client,
    invitation='ICLR.cc/2020/Conference/Reviewers/-/Conflict',
    label='Personal',
    tail='~Test_User1'
)
```

Since edges usually are very large in numbers, it is possible to get just the count of edges by using the function client.get\_edges\_count. Note that this only returns the count of edges visible to you.

```python
conflict_edges_count = client.get_edges_count(
    invitation='ICLR.cc/2020/Conference/Reviewers/-/Conflict',
    label='Personal'
)
```

Since most of the common tasks performed using _Edges_ require _Edges_ to be grouped, it’s also possible to query for already grouped _Edges_. Consider the following example that gets all reviewers grouped by papers they have conflicts with for the ICLR 2020 Conference.

{% hint style="info" %}
Note that in this case it's not necessary to pass `id`, `head` or `tail` as parameters. This is the advantage of using the `get_grouped_edges` method.
{% endhint %}

```python
grouped_conflict_edges = client.get_grouped_edges(
    invitation='ICLR.cc/2020/Conference/Reviewers/-/Conflict',
    groupby='head',
    select='tail,weight,label'
)
```

Consider the following example that gets all papers grouped by reviewers they are in conflict with for the ICLR 2020 Conference. It returns a list of lists of the {head, weight, label} of each conflict edge for that tail.

```python
grouped_conflict_edges = client.get_grouped_edges(
    invitation='ICLR.cc/2020/Conference/Reviewers/-/Conflict',
    groupby='tail',
    select='head,weight,label'
)
```

To group _Edges_, one must already know what the _edge.head_ and _edge.tail_ represent in an _Edge_ and that information can be seen from the _Edge_’s invitation.
