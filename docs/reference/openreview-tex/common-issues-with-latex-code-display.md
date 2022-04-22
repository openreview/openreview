# Common Issues with LaTeX Code Display

One possible reason is that the macro is not supported by MathJax. If this is the case then macro will appear as plain red text in with the rendered TeX.

Another possibility is that the field has Markdown enabled, but not all the backslashes in the TeX notation were escaped. This can lead to some layout problems, such as all the elements of a matrix appearing in 1 row instead of many. Similarly, underscores should also be escaped with a backslash when they are used at the beginning or the end of a word: '\\\_'.

For more details on the difference between OpenReview's TeX support and other systems, see [here](openreview-tex-support.md).
