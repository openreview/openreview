# How to Export All Reviews into a CSV

## API 2 Venues (most common)

To export all reviews for a given venue into a csv file:

1. If you have not done so, you will need to [install and instantiate the openreview-py client](../../getting-started/using-the-api/installing-and-instantiating-the-python-client.md).&#x20;
2. Retrieve all of the Reviews into the a `reviews` variable. Follow this link and complete all three steps for API 2 venues to [Get All the Reviews](how-to-get-all-reviews.md#venues-using-api-v2)&#x20;
3. Get the super review invitation. You can check out the [default review form](../../reference/default-forms/default-review-form.md#api-v2-json) if you need a reference. You'll need the content of the super invitation to create the headers for the .csv.

```python
invitation = client.get_invitation(f'{venue_id}/-/{review_name}')
content = invitation.edit['invitation']['edit']['note']['content']
print(content)
```

4. If you did not use any custom fields for the review form, we would expect a list like \["title", "review", "rating", "confidence"]. Now create a list of the review form fields found in the content.

```python
keylist = list(content.keys())
```

5. If you haven't already, `import csv`. Then iterate through the list of reviews stored in 'reviews' and for each one, append the values associated to the keys in your keylist. If a value does not exist for that key, put an empty string in its place. You may also want to know which submission each review is associated with. You can get the forum of each review, which corresponds to the forum page of its associated submission. For example, if a review's forum is aBcDegh, you could find that submission at https://openreview.net/forum?id=aBcDegh. To create a csv that includes the review forums, do this:

```python
with open('reviews.csv', 'w') as outfile:
    csvwriter = csv.writer(outfile, delimiter=',')
    # Write header 
    keylist.insert(0,'forum')
    t = csvwriter.writerow(keylist)
    for review in reviews:
        valueList = []
        valueList.append(review.forum)
        for key in keylist:
            if (key != 'forum'):
                if review.content.get(key)['value']:
                    valueList.append(review.content.get(key)['value'])
                else:
                    valueList.append('')
        s = csvwriter.writerow(valueList)
```

6. There should now be a .csv of exported reviews in the directory in which you are working.&#x20;

## API 1 Venues

Say you want to export all of the reviews for a given venue into a csv file.&#x20;

1. If you have not done so, you will need to [install and instantiate the openreview-py client](../../getting-started/using-the-api/installing-and-instantiating-the-python-client.md).&#x20;
2. Retrieve all of the Reviews into a `reviews` object following the instructions [here](how-to-get-all-reviews.md).&#x20;
3. Next, get the super review invitation. This is the overall review invitation which each of the Paper#/-/Official\_Review invitations are based off of, and it follows the format Venue/ID/-/Official\_Review.&#x20;

```python
invitation = client.get_invitation("<Your/Venue/Id/-/Official_Review>")
print(invitation.content)
```

4. Generate a list of the fields in the content in the Review invitation. For reference, this is what the default review invitation content looks like in JSON:&#x20;

```python
{
  "title": {
      "order": 1,
      "value-regex": ".{0,500}",
      "description": "Brief summary of your review.",
      "required": true
  },
  "review": {
      "order": 2,
      "value-regex": "[\\S\\s]{1,200000}",
      "description": "Please provide an evaluation of the quality, clarity, originality and significance of this work, including a list of its pros and cons (max 200000 characters). Add formatting using Markdown and formulas using LaTeX. For more information see https://openreview.net/faq",
      "required": true,
      "markdown": true
  },
  "rating": {
      "order": 3,
      "value-dropdown": [
          "10: Top 5% of accepted papers, seminal paper",
          "9: Top 15% of accepted papers, strong accept",
          "8: Top 50% of accepted papers, clear accept",
          "7: Good paper, accept",
          "6: Marginally above acceptance threshold",
          "5: Marginally below acceptance threshold",
          "4: Ok but not good enough - rejection",
          "3: Clear rejection",
          "2: Strong rejection",
          "1: Trivial or wrong"
      ],
      "required": true
  },
  "confidence": {
      "order": 4,
      "value-radio": [
          "5: The reviewer is absolutely certain that the evaluation is correct and very familiar with the relevant literature",
          "4: The reviewer is confident but not absolutely certain that the evaluation is correct",
          "3: The reviewer is fairly confident that the evaluation is correct",
          "2: The reviewer is willing to defend the evaluation, but it is quite likely that the reviewer did not understand central parts of the paper",
          "1: The reviewer's evaluation is an educated guess"
      ],
      "required": true
  }
}
```

so we would expect a list like \["title", "review", "rating", "confidence"]. This is how we get the list:

```python
keylist = list(review_invitation.reply['content'].keys())
```

5. If you haven't already, import csv. Then iterate through the list of reviews stored in 'reviews' and for each one, append the values associated to the keys in your keylist. If a value does not exist for that key, put an empty string in its place.&#x20;

```python
import csv
with open('reviews.csv', 'w') as outfile:
    csvwriter = csv.writer(outfile, delimiter=',')
    # Write header 
    t = csvwriter.writerow(keylist)
    for review in reviews:
        valueList = []
        for key in keylist:
            if review.content.get(key):
                if 'value' in review.content[key].keys():
                    valueList.append(review.content.get(key)['value'])
                else:
                    valueList.append(review.content.get(key))
            else:
                valueList.append('')
        s = csvwriter.writerow(valueList)
outfile.close()  
```

6. There should now be a csv of exported reviews in the directory in which you are working.&#x20;

### Adding submission information to the exported csv (both API2 and API1)

You may also want to know which submission each review is associated with. You can get the forum of each review, which corresponds to the forum page of its associated submission. For example, if a review's forum is aBcDegh, you could find that submission at https://openreview.net/forum?id=aBcDegh. To create a csv that includes the review forums, do this:

```python
with open('reviews.csv', 'w') as outfile:
    csvwriter = csv.writer(outfile, delimiter=',')
    # Write header 
    keylist.insert(0,'forum')
    t = csvwriter.writerow(keylist)
    for review in reviews:
        valueList = []
        valueList.append(review.forum)
        for key in keylist:
            if (key != 'forum'):
                if 'value' in review.content[key].keys():
                    valueList.append(review.content.get(key)['value'])
                else:
                    valueList.append(review.content.get(key))
            else:
                valueList.append('')
        s = csvwriter.writerow(valueList)
outfile.close()      
```

### Adding reviewer information to the exported csv (both API2 and API1)

You may also want to know the profile information of the reviewer associated with a review. First, create a mapping between the Reviewer ID (anonymous) and Profile ID:

```python
reviewerID_to_profileID =  {group.id.split('/')[-1]  : group.members[0] for group in submission_groups if '/Reviewer_' in group.id}
```

Then you can use this map to find the profile ID of the reviewer for a review:

```python
with open('reviews.csv', 'w') as outfile:
    csvwriter = csv.writer(outfile, delimiter=',')
    # Write header 
    keylist.insert(0,'profileID')
    t = csvwriter.writerow(keylist)
    for review in reviews:
        valueList = []
        valueList.append(reviewerID_to_profileID[review.signatures[0].split('/')[-1]])
        for key in keylist:
            if (key != 'profileID'):
                if review.content.get(key):
                    valueList.append(review.content.get(key))
                else:
                    valueList.append('')
        s = csvwriter.writerow(valueList)
outfile.close()    
```

If you want additional information about the reviewer, you can get their profile using the Profile ID (see [How to Get Profiles and Their Relations](how-to-get-profiles-and-their-relations.md) for more guidance).



