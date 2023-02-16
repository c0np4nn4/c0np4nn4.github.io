+++
title = "🔢 Division Algorithm"
date = 2022-12-05
+++


# The Division Theorem

<center class="quote">
If $a, b$ are any positive integers, <br />
there exist unique integers $q$ and $r$ such that
</center>

$$\textbf{b = aq + r} \quad \text{and} \quad \textbf{0} \le \textbf{r < a}$$

# Proof
### 1. Existence
`1)`
Let's say there exist set $A = \\{ b - na \mid n \in \mathbb{Z}, b - na \ge 0 \\}$

1) Set $A$ is not an ***empty set***, because if $n = 0$ then $A$ must have an element $b$
2) If $0 \in \mathbb{N}$ , then $A \subseteq \mathbb{N}$

Since the **well-ordering principle** states that<br />
<center class="quote">
Every non-empty set of positive integers contains "a least element"
</center>

Let's say <u>the least element</u> of $A$ is $r$.<br />
Then, there exist some $q$ such that $r = b - qa$ which means that
$$ b = qa + r \quad and \quad 0 \le r$$

`2)`
Let's say $r \ge a$, then

$$
\begin{align}
r - a &\ge 0 \\\\
b - qa - a &\ge 0 \\\\
b - (q + 1)a &\ge 0 \\\\
b - na &\ge 0 \\\\
\end{align}
$$

So, $r - a \in A$

Since, $a > 0$

$$r - a < r$$

It contradicts that *$r$ is <u>the least element</u> of set $A$*.

Therefore, $$r < a$$


### 2. Uniqueness
Let's say there exist integers $p$ and $s$ such that
$$b = ap + s$$

This assumption means that $\textbf{q}$ and $\textbf{r}$ from $\textbf{b = aq+ r}$ might not unique values.

Since, $b = \underline{aq + r} = \underline{ap + s}$
$$
\begin{align}
aq + r &= ap + s \\\\
aq - ap &= s - r \\\\
a(q - p) &= (s - r)
\end{align}
$$

Let's say if $p \neq q$, then
$$
\begin{align}
\left| a \right| &\le \left| q - p \right| \cdot \left| a \right| \\\\
\left| a \right| &\le \left| (q - p) \cdot a \right| \\\\
\left| a \right| &\le \left| (s - r) \right| \\\\
\end{align}
$$

Since, $r < a \\; \text{and} \\; s < a$
$$\left| (s - r) \right| < a$$

It clearly shows that *$p = q$* and therefore *$r = s$*


<!-- Math rendering -->
<script src='https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.5/latest.js?config=TeX-MML-AM_CHTML' async></script>
<script type="text/x-mathjax-config">
MathJax.Hub.Config({tex2jax: {inlineMath: [['$','$'], ['\\(','\\)']]}});
</script>
<!-- Math rendering -->

