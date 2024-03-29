# How to Export All Reviews into a CSV

API 2 Venues

To export all reviews for a given venue into a csv file:

1. If you have not done so, you will need to [install and instantiate the openreview-py client](../../getting-started/using-the-api/installing-and-instantiating-the-python-client.md).&#x20;
2. Retrieve all of the Reviews. Follow this link and complete all three steps for API 2 venues to [Get All the Reviews](how-to-get-all-reviews.md#venues-using-api-v2)&#x20;
3. Get the super review invitation. You can check out the [default review form](../../reference/default-forms/default-review-form.md#api-v2-json) if you need a reference. You'll need the content of the super invitation to create the headers for the csv.

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

6. There should now be a csv of exported reviews in the directory in which you are working.&#x20;

API 1 Venues

Say you want to export all of the reviews for a given venue into a csv file.&#x20;

1. If you have not done so, you will need to [install and instantiate the openreview-py client](../../getting-started/using-the-api/installing-and-instantiating-the-python-client.md).&#x20;
2. Retrieve all of the Reviews. Reviews generally follow the invitation Your/Venue/ID/-/Official\_Review. We can retrieve them by getting all of the direct replies to each submission and finding those with invitations ending in Official\_Review.&#x20;

Single blind venues can do this like so:&#x20;

```python
submissions = client.get_all_notes(invitation="Your/Venue/ID/-/Submission", details='directReplies')
reviews = [] 
for submission in submissions:
    reviews = reviews + [reply for reply in submission.details["directReplies"] if reply["invitation"].endswith("Official_Review")]
```

whereas double blind venues should replace "Submission" in the invitation with "Blind\_Submission":&#x20;

```python
submissions = client.get_all_notes(invitation="Your/Venue/ID/-/Blind_Submission", details='directReplies')
reviews = [] 
for submission in submissions: 
    reviews = reviews + [reply for reply in submission.details["directReplies"] if reply["invitation"].endswith("Official_Review")]
```

3\. Next, get the super review invitation. This is the overall review invitation which each of the Paper#/-/Official\_Review invitations are based off of, and it follows the format Venue/ID/-/Official\_Review.&#x20;

```python
invitation = client.get_invitation("<Your/Venue/Id/-/Official_Review>")
print(invitation.content)
```

4\. Generate a list of the fields in the content in the Review invitation. For reference, this is what the default review invitation content looks like in JSON:&#x20;

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

5\. If you haven't already, import csv. Then iterate through the list of reviews stored in 'reviews' and for each one, append the values associated to the keys in your keylist. If a value does not exist for that key, put an empty string in its place.&#x20;

```python
import csv
with open('reviews.csv', 'w') as outfile:
    csvwriter = csv.writer(outfile, delimiter=',')
    # Write header 
    t = csvwriter.writerow(keylist)
    for review in reviews:
        valueList = []
        for key in keylist:
            if review["content"].get(key):
                valueList.append(review["content"].get(key))
            else:
                valueList.append('')
        s = csvwriter.writerow(valueList)
outfile.close()  
```

6\. The previous example only exports the content fields of each review. You may also want to know which submission each review is associated with. You can get the forum of each review, which corresponds to the forum page of its associated submission. For example, if a review's forum is aBcDegh, you could find that submission at https://openreview.net/forum?id=aBcDegh. To create a csv that includes the review forums, do this:

```python
with open('reviews.csv', 'w') as outfile:
    csvwriter = csv.writer(outfile, delimiter=',')
    # Write header 
    keylist.insert(0,'forum')
    t = csvwriter.writerow(keylist)
    for review in reviews:
        valueList = []
        valueList.append(review["forum"])
        for key in keylist:
            if (key != 'forum'):
                if review["content"].get(key):
                    valueList.append(review["content"].get(key))
                else:
                    valueList.append('')
        s = csvwriter.writerow(valueList)
outfile.close()      
```

7\. There should now be a csv of exported reviews in the directory in which you are working.&#x20;
