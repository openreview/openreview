# How to override your venue homepage

On the request form for your venue, click on the ‘Revision’ button and modify the Homepage Override field, which expects valid JSON.

The instruction field of the JSON accepts HTML, allowing the formatting of the instructions section to be fully customized. All HTML should be validated to ensure that it will not break the layout of the page: [https://validator.w3.org/#validate\_by\_input](https://validator.w3.org/#validate\_by\_input)

Example:

```
{
  "title": "tinyML 2021",
  "subtitle": "First International Research Symposium on Tiny Machine Learning (tinyML)",
  "location": "Burlingame, CA",
  "deadline": "Submission Deadline: November 30, 2020 11:59pm AOE",
  "website": "https://tinyml.org/home/index.html",
  "instructions": "<h1>Important Dates</h1><li>Submission Deadline: November <strike>23</strike> 30, 2020 11:59pm AOE <b>(extended)</b></li><li>Author Notification: Jan 15th, 2021</li><li>Camera Ready: Feb 15th, 2021</li></br><h1>Submission Format</h1><li>Page limit is 6 to 8 pages, including references and any appendix material</li><li>Submissions must be blind for the double-blind review process</li><li>For paper formatting, please use the <a href=\"https://www.acm.org/publications/proceedings-template\">ACM SIGCONF template</a>.</li></br>"
}
```

which will be displayed as:

![homepage](https://openreview.net/images/faq-homepage-override.png)
