---
layout: post
comments: true
title: Test mathjax
use_math: true
---

# Using MathJax

\\[ \mathbf{X} = \mathbf{Z} \mathbf{P^\mathsf{T}} \\]

$$ \frac{1}{\left(1+\left(\frac{1}{1+\left(\frac{1}{1+2x}\right)}\right)\right)} $$


$$ \begin{align}
\nabla_{\theta} E_x[f(x)] &= \nabla_{\theta} \sum_x p(x) f(x) & \text{definition of expectation} \\
& = \sum_x \nabla_{\theta} p(x) f(x) & \text{swap sum and gradient} \\
& = \sum_x p(x) \frac{\nabla_{\theta} p(x)}{p(x)} f(x) & \text{both multiply and divide by } p(x) \\
& = \sum_x p(x) \nabla_{\theta} \log p(x) f(x) & \text{use the fact that } \nabla_{\theta} \log(z) = \frac{1}{z} \nabla_{\theta} z \\
& = E_x[f(x) \nabla_{\theta} \log p(x) ] & \text{definition of expectation}
\end{align}
$$
