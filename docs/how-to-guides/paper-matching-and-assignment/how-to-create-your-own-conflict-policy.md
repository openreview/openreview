# How to create your own Conflict Policy

The Paper Matching Setup allows for program organizers to select from three Conflict options: Default, NeurIPS, and No. However, program organizers can create their own conflict policy using the python client if they want something other than these three options. The policy can be as complex or as simple as you want.&#x20;

To create the policy you'll need to get the profile information you need via [get\_profile\_inf](https://github.com/openreview/openreview-py/blob/e59459b7292790e4551161f8f2eb9eeb61d4bfea/openreview/tools.py#L1445)[o](https://github.com/openreview/openreview-py/blob/e59459b7292790e4551161f8f2eb9eeb61d4bfea/openreview/tools.py#L1445) or utilize parts of the function to make your own. You'll want to include the information that you consider should be in conflict with another profile. When running the Paper Matching Setup, set Conflicts to No, and then compute the conflicts using the python client.

For example, suppose PCs only want to use the domains in the profile when computing conflicts:

{% code overflow="wrap" %}
```python
def own_policy(profile, n_years=None):
    domains = set()
    emails = set()
    relations = set()
    publications = set()

    ## Emails section
    for email in profile.content['emails']:
        # split email
        if '@' in email:
            domain = email.split('@')[1]
            domains.add(domain)
        else:
            print('Profile with invalid email:', profile.id, email)
            
    ## Institution section
    for history in profile.content.get('history', []):
        domains.add(domain)

    return {
        'id': profile.id,
        'domains': domains,
        'emails': emails,
        'relations': relations,
        'publications': publications
    }
```
{% endcode %}

Once you've created the policy you can [run conflicts](how-to-compute-conflicts-between-users.md) and then [post the edges](how-to-post-a-custom-conflict.md).

Running the function will look like:

{% code overflow="wrap" %}
```python
conflicts_for_reviewer = openreview.tools.get_conflicts(author_profiles, reviewer, policy=own_policy, n_years=None) # Returns a list of conflicts
```
{% endcode %}

We provide some default methods, but it's up to you how the conflicts are calculated.
