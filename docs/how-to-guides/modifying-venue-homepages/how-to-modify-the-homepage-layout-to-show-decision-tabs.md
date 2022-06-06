# How to modify the homepage layout to show decision tabs

Once decisions have been posted, you will see a [Post Decision Stage](../../reference/stages/post-decision-stage.md) button on the request form for your venue. Once you click on this button, you will be able to specify the name of each tab you want to include in the homepage in the form of a dictionary.

Note that each key must be a valid decision option. For example, a venue with the decision options Accept (Oral), Accept (Spotlight), Accept (Poster) and Reject could choose to hide the rejected papers tab as follows:

```
{
  "Accept (Oral)": "Accepted Oral submissions",
  "Accept (Spotlight)": "Accepted Spotlight submissions",
  "Accept (Poster)": "Accepted Poster submissions"
}
```
