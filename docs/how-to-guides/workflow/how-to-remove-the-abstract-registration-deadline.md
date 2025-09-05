# How to remove the Abstract Registration Deadline

When filling out the venue request form, some PCs include an Abstract Registration Deadline in their request. After the venue is deployed, they want to remove this deadline due to a change in their workflow. Unfortunately, removing this deadline is not as simple as deleting the date from the form.

To remove the deadline:

1. [Instantiate an API 1 client](../../getting-started/using-the-api/installing-and-instantiating-the-python-client.md).
2. Get the id of your request form, this is located in the URL.

<figure><img src="../../.gitbook/assets/Screenshot 2024-12-06 at 4.44.50 PM.png" alt="" width="375"><figcaption><p>URL of the request form containing the request form id.</p></figcaption></figure>

3. Run the code below.&#x20;

```python
request_form_id = ''

references = client.get_all_references(referent=request_form_id)
for ref in references:
    if 'abstract_registration_deadline' in ref.content:
        print("deadline removed from: ",ref.id)
        del ref.content['abstract_registration_deadline']
        client.post_note(ref)
```
