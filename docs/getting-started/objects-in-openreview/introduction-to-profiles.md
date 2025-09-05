# Introduction to Profiles

[Jump to QuickStart script for getting all profiles](introduction-to-profiles.md#quickstart-getting-all-profiles)

[Technical Reference for Profiles](../../reference/api-v2/entities/profile/)

**Profiles** in OpenReview represent the identity of users on the platform—such as authors, reviewers, and area chairs. Each profile serves as a record user's publications, affiliations, and roles within the OpenReview system.&#x20;

### **What’s in a Profile?**

A profile typically includes:

* **Profile ID:** This is the unique identifier in the system, in the format `~First_Last1` This is also sometimes referred to as a "tilde id".  While each tilde ID points to a single profile, a profile may have multiple tilde IDs associated with it as a result of adding an alternate name or merging profiles.
* **Name(s)**: Full name and any alternate names (e.g., name changes, nicknames).
* **Email(s)**: Verified email addresses associated with the user. Only the email domains are publicly displayed- even PCs will not see the full emails for users submitting to their venue. Note, if you need to get the full author emails, refer to the instructions [here](../../how-to-guides/communication/how-to-get-email-addresses.md).
* **Affiliations**: Work history or institutional associations.
* **Publications**: Papers the user has authored or co-authored.

### **Getting a Profile or Profiles**

You can get one or more profiles using the Python API using the function `openreview.tools.get_profiles()` . This function takes a list of profiles or emails, and for each item in the list, returns one of the following: It is possible to query a profile either using the profile's tilde id, or using any confirmed email to the profile&#x20;

```python
openreview.tools.get_profiles(client_v2, <LIST_OF_PROFILE_IDS_OR_EMAIL>)
```

To get multiple profiles, you would use the function `openreview.tools.get_profiles()` . This function takes a list of profiles or emails, and for each item in the list, returns a dictionary with the following:&#x20;

```python
profile_id_list = []
profiles = openreview.tools.get_profiles(client_v2,profile_id_list,as_dict=True)

```

Other arguments that can be used to get additional informations along with the profiles are:&#x20;

* `with_publications`&#x20;
* `with_relations`&#x20;
* `with_preferred_emails`&#x20;

### **Structure of Profiles**

Profiles are an OpenReview object that contains properties. The main three properties that you can expect to interact with are:  `id` , `state`, and `content`. For more details about each of these fields, see [here](../../reference/api-v2/entities/profile/fields.md). Several of the fields of `content` include lists or lists of dictionaries, which means that it is necessary to understand the structure of the profile in order to get the profile information.in order to access this data, it is Because you need to flatten the dictionary to create the fields, then extract the content, similarly to how the submission content was extracted. The original profile information looks something like this:

```
{'active': True,
 'state': 'Active'
 'content': {'emails': ['name@university.edu'],
             'emailsConfirmed': ['name@university.edu'],
             'history': [{'end': None,
                          'institution': {'country': 'US',
                                          'domain': 'university.edu'},
                          'position': 'PhD Student',
                          'start': 2017}],
             'homepage': 'https://test.com',
             'names': [{'fullname': 'First Last',
                        'preferred': True,
                        'username': '~First_Last2'}],
             'preferredEmail': 'name@university.edu',
             'relations': []},
 'id': '~First_Last2',
 ...<other metacontent>...
 
 }
```

To get a tabular format, it is necessary to flatten the profile. After flattening (sample code below) a profile would look like this:

<table><thead><tr><th width="51.8984375"></th><th>preferredEmail</th><th>homepage</th><th>emails_0</th><th>names_0_preferred</th><th>names_0_fullname</th><th>names_0_username</th><th>history_0_position</th><th>history_0_start</th><th>history_0_end</th><th>history_0_institution_country</th><th>history_0_institution_domain</th><th>emailsConfirmed_0</th><th>profile_id</th></tr></thead><tbody><tr><td>0</td><td>name@university.edu</td><td>https://test.com</td><td>name@university.edu</td><td>True</td><td>First Last</td><td>~First_Last2</td><td>PhD Student</td><td>2017</td><td>None</td><td>US</td><td>university.edu</td><td>name@university.edu</td><td>~First_Last2</td></tr></tbody></table>



There will be multiple columns for some profile fields recording each of the entries, for example: `names_0_preferred, names_0_fullname`. Because profiles have different numbers of affiliations in their profile, some of these columns will be null for some profiles.&#x20;

### QuickStart: Getting All Profiles

The code below takes a list of profile IDs or emails, and returns a DataFrame with all of the profile information in it in a tabular format.&#x20;

```python
client_v2 = #connect to the OpenReview Client (API2) with your credentials

from collections.abc import MutableMapping

list_of_profile_ids = []
profile_list = openreview.tools.get_profiles(client_v2,list_of_profile_ids)


def flatten_dict(d, parent_key='', sep='_'):
    """
    Recursively flattens a dictionary, concatenating nested keys.
    """
    items = []
    for k, v in d.items():
        new_key = f"{parent_key}{sep}{k}" if parent_key else k
        if isinstance(v, MutableMapping):
            items.extend(flatten_dict(v, new_key, sep=sep).items())
        elif isinstance(v, list):
            for i, elem in enumerate(v):
                # Handle lists of dictionaries by adding an index
                if isinstance(elem, MutableMapping):
                    items.extend(flatten_dict(elem, f"{new_key}_{i}", sep=sep).items())
                else:
                    # Just add the element if it's not a dictionary
                    items.append((f"{new_key}_{i}", elem))
        else:
            items.append((new_key, v))
    return dict(items)

def extract_content(d):
    flattened = flatten_dict(d.content)
    content = {k: v for k, v in flattened.items()}
    content['profile_id'] =d.id
    return(content)


#Create a DataFrame with the flattened profile content + profile ID
profile_df = pd.DataFrame([extract_content(note) for note in profile_list)

#extract the columns you want included in the data
relevant_columns = ['profile_id'] + [c for c in profile_df.columns if 'history_0' in c] 
profile_df_subset = profile_df[relevant_columns]
```

Once the DataFrame is created, it is possible to create a CSV with this data, or merge it with other OpenReview data. See [here](../../how-to-guides/data-retrieval-and-modification/how-to-loop-through-accepted-papers-and-print-the-authors-and-their-affiliations.md) for examples on how to combine profile with submission data.&#x20;
