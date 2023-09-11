# How to add formulas or use mathematical notation

OpenReview supports TeX and LaTeX notation in many places throughout the site, including forum comments and reviews, paper abstracts, and venue homepages. To indicate that some piece of text should be rendered as TeX, use the delimiters `$...$` for inline math or `$$...$$` for displayed math. For example, this raw text:

```
This is what the Pythagorean theorem $x^2 + y^2 = z^2$ looks like.

Here is an example of multiple integrals:

$$
\iiiint_V \mu(t,u,v,w) \\,dt\\,du\\,dv\\,dw
$$
```

will be displayed as:

![MathJax example image](https://openreview.net/images/faq-mathjax-example.png)

For more information on LaTeX notation we recommend [Overleaf's Guide](https://www.overleaf.com/learn/latex/Mathematical\_expressions). For more information about OpenReview TeX support, go [here](../../reference/openreview-tex/).&#x20;
