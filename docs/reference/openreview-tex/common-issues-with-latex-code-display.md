# Common Issues with LaTeX Code Display

If your TeX notation is not rendering the way you expect, work through the following:

* **The macro may not be supported by MathJax.** If this is the case then macro will appear as plain red text in with the rendered TeX. MathJax implements only math-mode macros, so text-mode macros and environments such as `\emph` or `\begin{enumerate}...\end{enumerate}` will not render. See [Math-mode macros only](openreview-tex-support.md#math-mode-macros-only).
* **The command may be restricted.** MathJax runs with its `safe` extension enabled, so commands that set arbitrary links, styles, font sizes, or element classes and ids are filtered out even when MathJax implements them. See [Some commands are restricted](openreview-tex-support.md#some-commands-are-restricted).
* **The backslashes may not be escaped.** If the field has Markdown enabled, but not all the backslashes in the TeX notation were escaped, Markdown strips them before the TeX is parsed. This can lead to some layout problems, such as all the elements of a matrix appearing in 1 row instead of many. Similarly, underscores should also be escaped with a backslash when they are used at the beginning or the end of a word: '\\\_'. See [TeX in a Markdown-enabled field](openreview-tex-support.md#tex-in-a-markdown-enabled-field).
* **The delimiters may be missing.** Text is only rendered as TeX when it sits inside `$...$` for inline math or `$$...$$` for displayed math. See [Delimiters](openreview-tex-support.md#delimiters).
* **An HTML tag may be interfering.** Do not mix HTML tags with TeX: text that begins with `<` is treated as an HTML block, and the whole field can lose its Markdown formatting. Where a `<` or `>` has to appear inside the math delimiters, add spaces around it so it is not read as an open tag. See [TeX inside an HTML document](openreview-tex-support.md#tex-inside-an-html-document).

For the full picture of how OpenReview's TeX support differs from other systems, see [OpenReview TeX support](openreview-tex-support.md).
