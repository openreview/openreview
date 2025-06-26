# How to create a custom submission export

While exports are available for submissions in the UI, if you want to create an export with information not available from the default export, you can use the Python client to do so.

### Get Submissions

1. If you have not done so, you will need to [install and instantiate the openreview-py client](../../getting-started/using-the-api/installing-and-instantiating-the-python-client.md).&#x20;
2. [Get the submissions](broken-reference) that you want to export.

### Extract data

There are a couple methods that you can use to extract information from the submissions. Which method you use depends on how many papers you have, how many fields you want to extract, and personal preference.&#x20;

No matter the method you use, it is important to understand the structure of the data- both [Notes](../../getting-started/using-the-api/objects-in-openreview/introduction-to-notes.md) (Submissions) and [Profiles](../../getting-started/using-the-api/objects-in-openreview/introduction-to-profiles.md) have nested dictionaries stored within the `content` property. Functionally, what that means in this case is:

1. If you query for a field that doesn't exist, the code will exit with an error. Rather than getting the value directly, I recommend using the `.get(<fieldname>,<null_value>)` dictionary method. This will return the value of the field if it exists, and another value if the field doesn't exist, rather than giving an error. Typically, the null value should match the type of the expected output.&#x20;
2. For querying most fields in a submission/profile, you will need to look within the `content` property (see example in 3. below)
3. For submissions, to get a value from the submission, you will need to use the format `submission.content[<fieldname>].get(['value'],<null_value>]`&#x20;
4. For profiles, there may be multiple nested items in the content. For example, a user with multiple past affiliations will have a list of dictionaries for their history, with the current affiliation listed first. Profiles may have different lengths for these values, which is important to keep in mind when extracting data.&#x20;

#### Method 1: Loop

The simplest method is to loop through all submissions and extract the relevant information. Here we simply print the data, but you could also write it to a csv. Below is an example where the submission number, title, author names, and author current affiliation are extracted from the data.&#x20;

```python
for submission in submissions:
    print(submission.number, submission.content['title'].get(['value'],'')) 
    author_profiles = openreview.tools.get_profiles(client, submission.content['authorids'].get(['value'],''))
    for author_profile in author_profiles:
        print(author_profile.get_preferred_name(pretty=True), author_profile.content.get('history', [{}])[0])
```

#### Method 2: Table

The second method is to generate a DataFrame with all of the data in the submissions, then select the relevant fields from the table for extraction. This is helpful if you want many fields from the submission, or if you have many submissions. Below is an example where the submission number, title, author names, and author current affiliation are extracted.&#x20;



```python
import pandas as pd

def extract_content_values(note):
    content = {k: v.get('value', None) for k, v in note.content.items()}
    content['number'] = note.number

df = pd.DataFrame([extract_content_values(note) for note in submissions])

#Subset for the relevant fields
subset_df = df[['number','title','authors','authorids']]

#export data
subset_df.to_csv('submission_information.csv', index=False)
```

If all you need is submission information, you can export the data at this point. If you want to integrate this information with other fields, such as author information, reviews, etc.&#x20;

### Combining Submissions with Other information

First you will need to get the relevant data in a tabular format in order to be able to combine the DataFrames.

#### Profiles

You can get a list of all profile IDs for authors from your newly created DataFrame.&#x20;

```python
#get a list of all authors
all_author_ids = set(id for ids in subset_df['authorids'] for id in ids)

profiles = openreview.tools.get_profiles(client_v2,all_author_ids)
```

Once you have a list of profiles, refer to the script at the end of [this](../../getting-started/using-the-api/objects-in-openreview/introduction-to-profiles.md) page to create a DataFrame.&#x20;

The profile and Submission DataFrame can then be combined in a variety of ways.&#x20;

#### Example: Check if any authors have a particular trait

In this case, we are checking if any authors have the 'example.com' domain.

```python
domain_to_check = 'example.com'

#create a row that checks for that domain across all columns in the data
profile_df['contains_domain'] = profile_df_subset.apply(lambda row: any(
    row.astype(str).str.contains(domain_to_check, case=False, na=False)
), axis=1)

#map to a column ['any_has_domain'] that is True if any author has that domain in their history
def map_has_domain(df_notes, df_profiles, id_col='authorids', profile_id_col='profile_id', domain_col='contains_domain'):
    # Build lookup: profile_id → has_domain (from df_profiles)
    has_domain_lookup = df_profiles.set_index(profile_id_col)[domain_col].to_dict()

    # For each list of author IDs, return True if any have has_domain = True
    def any_has_domain(id_list):
        return any(has_domain_lookup.get(pid, False) for pid in id_list)

    df_notes['any_has_domain'] = df_notes[id_col].apply(any_has_domain)
    return df_notes

mapped_df = map_has_domain(df, profile_df)

#subset for only submissions with that domain
affiliated_df = mapped_df.loc[df_notes['any_has_domain'] == True]
affiliated_df.shape
```

#### Example: Label profiles with submission number

This example will add the submission number to each profile in the profile DataFrame:

```python
# Step 1: Explode the 'authorids' list into separate rows
df_exploded = subset_df.explode('authorids')

# Step 2: Merge df_profiles with the exploded df_submission
df_merged = pd.merge(profile_df_subset, df_exploded[['authorids','number']], left_on='profile_id', right_on='authorids', how='left')

# Optional: Drop the 'authorids' column if it's no longer needed
df_merged = df_merged.drop(columns='authorids')

```



