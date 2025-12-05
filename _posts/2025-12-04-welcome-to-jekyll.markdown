---
layout: post
title:  "Welcome to Jekyll!"
date:   2025-12-04 17:22:48 -0800
categories: jekyll update
---

<!-- omit in toc -->
# hEADER LOL

- [1. CONCEPT 1](#1-concept-1)
- [2. EMBEDDED video lmao](#2-embedded-video-lmao)

*added by Markdown All In One. take it up to them if you have any complaints*

## 1. CONCEPT 1

You’ll find this post in your `_posts` directory. Go ahead and edit it and re-build the site to see your changes. You can rebuild the site in many different ways, but the most common way is to run `jekyll serve`, which launches a web server and auto-regenerates your site when a file is updated.

Jekyll requires blog post files to be named according to the following format:

`YEAR-MONTH-DAY-title.MARKUP`

Where `YEAR` is a four-digit number, `MONTH` and `DAY` are both two-digit numbers, and `MARKUP` is the file extension representing the format used in the file. After that, include the necessary front matter. Take a look at the source for this post to get an idea about how it works.

Jekyll also offers powerful support for code snippets:

{% highlight ruby %}
def print_hi(name)
  puts "Hi, #{name}"
end
print_hi('Tom')
#=> prints 'Hi, Tom' to STDOUT.
{% endhighlight %}

**some wonderful python code :)**:

```python
print("ur mom lol")
```

will this be unprocessed katex code?? let's find out. okay, the objective function of GraphSLAM is:

$$
\begin{align}
J_{GraphSLAM} = & \quad x^T_0 \Omega_0 x_0                \notag \\
                & + \sum_{t} \bigg[
                  \Big( x_t - g(u_t, x_{t-1}) \Big)^T
                  R^{-1}_t
                  \Big( x_t - g(u_t, x_{t-1}) \Big)
                \bigg]                                    \notag \\
                & + \sum_{t} \sum_{i} \bigg[
                  \Big( z^i_t - h(x_t, m_{c^i_t}) \Big)^T
                  Q^{-1}_t
                  \Big( z^i_t - h(x_t, m_{c^i_t}) \Big)
                \bigg] \\
\end{align}
$$

meanwhile, this is a matrix. why am I making so much effort for a bit:

$$
\begin{align}
  \begin{pmatrix}
    a &b \\
    c &d
  \end{pmatrix}
\end{align}
$$

## 2. EMBEDDED video lmao

<iframe
  width="959" height="539"
  src="https://www.youtube.com/embed/sRW00qtj8A0"
  title="Music From &#39;The Humanist Report&#39; | The Full Collection"
  frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin"
  allowfullscreen>
</iframe>

- hey `Tremendous` is actually really good.
  - "I will eat your ass." - Alex Jones


Check out the [Jekyll docs][jekyll-docs] for more info on how to get the most out of Jekyll. File all bugs/feature requests at [Jekyll’s GitHub repo][jekyll-gh]. If you have questions, you can ask them on [Jekyll Talk][jekyll-talk].

[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/
