---
layout: post
title: Beyond 3.14
date: 2023-08-06
description: Fascinating ways of calculating $$\pi$$ upto 5000 digits. 
tags: python code math
categories: math-for-fun
#toc:
#  sidebar: left
---

Hey there, fellow math and computer enthusiasts! Today, we're diving into one of the most captivating and profound constants in mathematics - Pi ($$\pi$$). As enthusiasts of both math and computing, we understand the extraordinary significance of this simple, yet infinitely complex number. So, let's embark on a journey to unravel the mysteries of pi and discover different ways of calculating $$\pi$$.

You may have wondered how people compute the value of $$\pi$$ accurately, and how do they know that they are accurate. Then you are at right place, today we will unravel the mysteries of convergence and error metrics.

{% include figure.html path="assets/img/pi_for_fun/pi_comic.jpg" class="img-fluid rounded z-depth-1" %}
<blockquote>
"Truth is ever to be found in simplicity, and not in the multiplicity and confusion of things." – Isaac Newton
</blockquote>

### Introduction
Pi ($$\pi$$), a symbol revered by mathematicians and enthusiasts alike, encapsulates the profound link between the geometry of circles and the elegance of numbers. It's no ordinary constant; it's a mathematical enigma that carries the weight of countless equations and the beauty of infinite precision.

Defined as the ratio of a circle's circumference to its diameter, pi unveils the mystery of circles in a single equation:

$$
\pi = \frac{Circumference}{Diameter} 
$$

However, pi's allure goes far beyond a simple fraction. Its decimal representation, an unending series of digits without repetition, makes it an irrational number. Let's emphasize this infinite beauty with the well-known approximation:

$$
\pi \approx 3.14159 
$$

Yet, pi's significance reverberates throughout diverse mathematical and scientific domains. In geometry, pi enables us to calculate the areas and volumes of circles and spheres. In trigonometry, pi influences the sine and cosine functions, laying the foundation for understanding waves and oscillations. And when we venture into calculus, pi emerges yet again, intertwining with limits, integrals, and derivatives in intricate equations.

Beyond these equations, pi's pervasive presence extends into the realms of physics, engineering, and beyond. From calculating the motion of planets to designing structures that stand the test of time, pi's role is undeniable.

### Origin and Historical Significance of $$\pi$$
Certainly, here's a concise one-line summary of each historical point related to the origin and history of $$\pi$$:

1. **Ancient Egypt and Babylon (circa 1900–1600 BCE)**: Early approximations of $$\pi$$ as 3.125.

2. **Ancient Greece (circa 250 BCE)**: Archimedes' geometric approach establishes bounds on $$\pi$$ between 3.1408 and 3.1429.

3. **India (circa 5th century CE)**: Aryabhata approximates $$\pi$$ as 62832/20000, or approximately 3.1416.

4. **Islamic Golden Age (circa 9th-13th centuries CE)**: Al-Khwarizmi and Al-Kashi contribute with polygonal approximations and precise calculations to 16 decimal places.

5. **Europe (Medieval and Renaissance Periods)**: Fibonacci and Ludolph van Ceulen make noteworthy contributions, with Ludolph calculating $$\pi$$ to 35 decimal places.

### Modern Methods of Calculating Pi: Python Implementation
Let's explore some modern methods used to calculate pi and provide Python implementations to demonstrate these techniques and calculating $$\pi$$ upto 5000 digits.

#### Calculating $$\pi$$ using Machin-like formula
One of the most well-known methods for calculating pi is the Machin-like formula, discovered by John Machin in 1706. This formula has been in use for around 300 years and converges quickly, making it suitable for the calculation of pi. The formula relates pi to arctangent (inverse tangent) values, as shown below:

$$
{\frac  {\pi }{4}}=4\arctan {\frac  {1}{5}}-\arctan {\frac  {1}{239}} 
$$

To calculate pi using this formula, we need to approximate the arctangent values. The Taylor's series expansion for arctangent is:

$$
arctan (x)=\sum _{n=0}^{\infty }{\frac {(-1)^{n}}{2n+1}}x^{2n+1}=x-{\frac {x^{3}}{3}}+{\frac {x^{5}}{5}}-{\frac {x^{7}}{7}}+\cdot
$$

Using this equations, we can write a python function to calculate the arctan approximated up to n terms. 

```py
from decimal import Decimal as dec

def arctan_approx(x,n):
    x = dec(x)
    total = dec(0)
    # for -1 <= x <= 1
    for i in range(n):
        sign = -1 if i%2 else 1
        f = dec(2*i + 1)
        denom = dec(f * dec(x**f))
        total += dec(sign / denom)
    return total
```

Using the properties of arctan, we can also represent pi as:

$$
{\frac  {\pi }{4}}=\arctan {\frac  {1}{3}}+\arctan {\frac  {1}{2}} 
$$



#### Calculating $$\pi$$ using Wallis Product

#### Calculating $$\pi$$ using Monte Carlo Method

#### Calculating $$\pi$$ using Viète's formula
