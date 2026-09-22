# OpenReview TeX support

OpenReview renders TeX with [MathJax](https://docs.mathjax.org/en/latest/index.html), with some specific considerations, which are described below:

## Delimiters

Mark the text that should be rendered as TeX with one of these pairs:

|                | Delimiters |
| -------------- | ---------- |
| Inline math    | `$...$`    |
| Displayed math | `$$...$$`  |

Anything outside the delimiters is treated as ordinary text or Markdown. Displayed math is aligned to the left rather than centred. See [How to add formulas or use mathematical notation](../../how-to-guides/submissions-comments-reviews-and-decisions/how-to-add-formulas-or-use-mathematical-notation.md) for a full example.

## Math-mode macros only

The TeX input processor implements only the math-mode macros of TeX and LaTeX, not the text-mode macros. So, for example, MathJax does not implement `\emph` or `\begin{enumerate}...\end{enumerate}` or other text-mode macros or environments. You must use Markdown (if enabled) to handle such formatting tasks.

## Some environments are supported only in part

Some features in MathJax might be limited. For example, MathJax only implements a limited subset of the array environment’s preamble; i.e., only the l, r, c, and | characters alongside : for dashed lines — everything else is ignored.

## Some commands are restricted

MathJax runs with its `safe` extension enabled, because your notation is displayed to other people on a shared page. Constructs that set arbitrary links, styles, font sizes, or element classes and ids are filtered out or restricted. A command can therefore be stripped here even though MathJax implements it and it works in your own LaTeX build.

## TeX in a Markdown-enabled field

When adding TeX content to a Markdown enabled field, it is important that all backslashes (\\) are escaped (i.e. replaced with \\\\) to prevent Markdown from stripping the backslashes before the TeX notation is parsed. If Markdown is not enabled, this is not necessary.

The same formula therefore looks different depending on the field:

```
Markdown not enabled:  $\frac{a}{b}$
Markdown enabled:      $\\frac{a}{b}$
```

Underscores need the same treatment when they appear at the beginning or the end of a word.

## TeX inside an HTML document

Keep in mind that your mathematics is part of an HTML document, so you need to be aware of the special characters used by HTML as part of its markup. There cannot be HTML tags within the math delimiters as TeX-formatted math does not include HTML tags. Make sure to add spaces around any `<` or `>` symbols to ensure they are not treated as open tags.

It is best not to mix HTML tags with TeX at all. Text that begins with `<` is treated as an HTML block, and the whole field can lose its Markdown formatting as a result.
