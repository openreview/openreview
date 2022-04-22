# Exporting all Reviews into a CSV

#### Exporting all Reviews into a CSV&#x20;

Say you want to export all of the reviews for a given venue into a csv file.&#x20;

1. If you have not done so, you will need to [install and instantiate the openreview-py client](../installing-and-instantiating-the-python-client.md).&#x20;
2. Use iterget\_notes to retrieve all notes with the __ Review invitation for your venue of interest. Most review invitations follow the format Venue/ID/Paper#/-/Official_\__Review.&#x20;

```
reviews = list(openreview.tools.iterget_notes(client, invitation = "Venue/ID/Paper.*/-/Official_Review"))
```

2\. Next, get the super review invitation. This is the overall review invitation which each of the Paper#/-/Official\_Review invitations are based off of, and it follows the format Venue/ID/-/Official_\__Review. &#x20;

```
review_invitation = client.get_invitation('Venue/ID/-/Official_Review')
```

3\. Generate a list of the fields in the content in the Review invitation. For reference, this is what the default review invitation content looks like in JSON:&#x20;

```
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

```
keylist = list(review_invitation.reply['content'].keys())
```

4\. If you haven't already, import csv. Then iterate through the list of reviews stored in 'reviews' and for each one, append the values associated to the keys in your keylist. If a value does not exist for that key, put an empty string in its place.&#x20;

```
import csv
with open('reviews.csv', 'w') as outfile:
    csvwriter = csv.writer(outfile, delimiter=',')
    # Write header 
    t = csvwriter.writerow(keylist)
    for review in reviews:
        valueList = []
        for key in keylist:
            if review.content.get(key):
                valueList.append(review.content.get(key))
            else:
                valueList.append('')
        s = csvwriter.writerow(valueList)
outfile.close()   
```

5\. The previous example only exports the content fields of each review. You may also want to know which submission each review is associated with. You can get the forum of each review, which corresponds to the forum page of its associated submission. For example, if a review's forum is aBcDegh, you could find that submission at https://openreview.net/forum?id=aBcDegh. To create a csv that includes the review forums, do this:

```
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
                if review.content.get(key):
                    valueList.append(review.content.get(key))
                else:
                    valueList.append('')
        s = csvwriter.writerow(valueList)
outfile.close()   
```

6\. There should now be a csv of exported reviews in the directory in which you are working.&#x20;
