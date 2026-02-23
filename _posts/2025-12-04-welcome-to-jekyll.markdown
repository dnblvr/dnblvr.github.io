---
layout: post
title: "Test Implementation!"
date: 2025-12-04 17:22:48 -0800
categories: jekyll update
tags: hi
toc: true
---

<!-- omit in toc -->
# Contents

<!-- markdownlint-disable MD033 -->
<!-- markdownlint-disable MD025 -->

- [1. All about Jekyll](#1-all-about-jekyll)
- [2. Code](#2-code)
  - [2.1. Ruby](#21-ruby)
  - [2.2. Python](#22-python)
- [3. TeX Equations](#3-tex-equations)
- [4. Mermaid Diagrams](#4-mermaid-diagrams)
- [5. Embedded Videos](#5-embedded-videos)

---

<br>

## 1. All about Jekyll

You’ll find this post in your `_posts` directory. Go ahead and edit it and re-build the site to see your changes. You can rebuild the site in many different ways, but the most common way is to run `jekyll serve`, which launches a web server and auto-regenerates your site when a file is updated.

Jekyll requires blog post files to be named according to the following format:

`YEAR-MONTH-DAY-title.MARKUP`

Where `YEAR` is a four-digit number, `MONTH` and `DAY` are both two-digit numbers, and `MARKUP` is the file extension representing the format used in the file. After that, include the necessary front matter. Take a look at the source for this post to get an idea about how it works.

incidentally, I am using this [wonderful site][] to check my progress and follow up.

[wonderful site]: <https://carpentry.library.ucsb.edu/2022-01-31-ucsb-webpub-online/05-starting-jekyll/> "Web Publishing with GitHub Pages: Starting With Jekyll - UCSB"

## 2. Code

Jekyll also offers powerful support for code snippets [^1].

[^1]: this is a note

### 2.1. Ruby

```ruby
def print_hi(name)
  puts "Hi, #{name}"
end
print_hi('Tom')
#=> prints 'Hi, Tom' to STDOUT.
```

### 2.2. Python

```python
def hello(name: str = "you shmuck") -> None:
  print(f"hello, {name.upper()}!")

hello("you")
```

## 3. TeX Equations

```TeX
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
```

**Output**:

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

Meanwhile, this is a matrix:

$$
\begin{align}
  \begin{pmatrix}
    a &b \\
    c &d
  \end{pmatrix}
\end{align}
$$

## 4. Mermaid Diagrams

```
~~~mermaid
flowchart LR

  A1(("Start"))
  A1 --> B1("count 100 ms") --> C1("Generate<br>timer interrupt")
  C1("Generate<br>timer interrupt") --> B1

  B1@{ shape: delay }

  A(("start")) --> B("WaitForInterrupt()")
  %% B --> C{"interrupt<br>generated?"}
  %% C -- true --> D("toggle **all**<br>RXIFG flags")
  B --> D("toggle **all**<br>RXIFG flags")
  %% C -- false --> B
  D --> E[/"Perform tasks"/]
  E --> F{"if every<br>nth pose"}
  F -- true --> G("Perform 4 graph<br>optimizations")
  F -- false --> H[/"send to point-cloud map<br>to Processing sketch"/]
  G --> H
  H --> B

  B@{ shape: delay }

~~~
```

**Output**:

```mermaid
flowchart LR

  A1(("Start"))
  A1 --> B1("count 100 ms") --> C1("Generate<br>timer interrupt")
  C1("Generate<br>timer interrupt") --> B1

  B1@{ shape: delay }

  A(("start")) --> B("WaitForInterrupt()")
  %% B --> C{"interrupt<br>generated?"}
  %% C -- true --> D("toggle **all**<br>RXIFG flags")
  B --> D("toggle **all**<br>RXIFG flags")
  %% C -- false --> B
  D --> E[/"Perform tasks"/]
  E --> F{"if every<br>nth pose"}
  F -- true --> G("Perform 4 graph<br>optimizations")
  F -- false --> H[/"send to point-cloud map<br>to Processing sketch"/]
  G --> H
  H --> B

  B@{ shape: delay }

```

We can also use the HTML tags `<div class="mermaid"></div>`.

```HTML
<div class="mermaid">
flowchart LR

  ...

</div>
```

## 5. Embedded Videos

<iframe
  width="480" height="270"
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
