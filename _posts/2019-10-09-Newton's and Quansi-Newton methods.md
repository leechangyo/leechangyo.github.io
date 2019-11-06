---
layout: post
title: 9. Newton's and Quansi-Newton methods
category: Optimization method
tag: Optimization method
---

# 1. Features of Newton’s method
- Use both first and second derivatives.
- Based on local quadratic approximation to the objective function.
- Requires a positive Hessian to work.
- Converge quadratically under conditions.

# 2. Derivation of Newton’s method

<a href="https://postimg.cc/McpsnryG"><img src="https://i.postimg.cc/43zjFDyp/Capture.png" width="700px" title="source: imgur.com" /><a>
<a href="https://postimg.cc/xccYrtkL"><img src="https://i.postimg.cc/3JZwvqYf/Capture.png" width="700px" title="source: imgur.com" /><a>
<a href="https://postimg.cc/5QF3LX97"><img src="https://i.postimg.cc/cCFbq3pS/Capture.png" width="700px" title="source: imgur.com" /><a>
<a href="https://postimg.cc/svHWsjV1"><img src="https://i.postimg.cc/VkmW9vp9/Capture.png" width="700px" title="source: imgur.com" /><a>
<a href="https://postimg.cc/7fz4YX47"><img src="https://i.postimg.cc/9F8QbNNx/Capture.png" width="700px" title="source: imgur.com" /><a>
<a href="https://postimg.cc/FdqKwRbQ"><img src="https://i.postimg.cc/RhCn6JNq/Capture.png" width="700px" title="source: imgur.com" /><a>
<a href="https://postimg.cc/CBjFrNKb"><img src="https://i.postimg.cc/9MkrMg5g/Capture.png" width="700px" title="source: imgur.com" /><a>
<a href="https://postimg.cc/NKmBjD1S"><img src="https://i.postimg.cc/nhwzTwMF/Capture.png" width="700px" title="source: imgur.com" /><a>
<a href="https://postimg.cc/zyXFJ2ZX"><img src="https://i.postimg.cc/vZ5qtC09/Capture.png" width="700px" title="source: imgur.com" /><a>
<a href="https://postimg.cc/p5MXZSYf"><img src="https://i.postimg.cc/KjRkZS1H/Capture.png" width="700px" title="source: imgur.com" /><a>
<a href="https://postimg.cc/ZCkV8LkJ"><img src="https://i.postimg.cc/fTyphHBS/Capture.png" width="700px" title="source: imgur.com" /><a>
<a href="https://postimg.cc/tsJZ7LrJ"><img src="https://i.postimg.cc/K8rnpbBP/Capture.png" width="700px" title="source: imgur.com" /><a>
<a href="https://postimg.cc/CR8SBHqy"><img src="https://i.postimg.cc/W1W3y5p4/Capture.png" width="700px" title="source: imgur.com" /><a>

- Rate of convergence is a measure of how fast the difference between the solution point and its estimates goes to zero. Faster algorithms usually use second-order information about the problem functions when calculating the search direction.

# 3. Two issues for Newton’s method

- When the dimension n is large, obtain F(x^(k)) and its inverse is computationally expensive => consider quasi-newton’s method
- Indefinite Hessian: the direction is not necessarily descending

<a href="https://postimg.cc/r0K8MkgC"><img src="https://i.postimg.cc/NMxL8GHV/Capture.png" width="700px" title="source: imgur.com" /><a>

# 4. Quasi Newton Method
- Big O notation is a mathematical notation that describes the limiting behavior of a function when the argument tends towards a particular value or infinity.
- Newton's method is one of the more successful algorithms for optimization
- If it converges, it has a quadratic order of convergence.
- However, as pointed out before, for a general nonlinear objective function, convergence to a solution cannot be guaranteed from an arbitrary initial point x^(0). In general, if the initial point is not sufficiently close to the solution, then the algorithm may not possess the descent
property
- Recall that the idea behind Newton's method is to locally approximate the function f being minimized, at every iteration, by a quadratic function. The minimizer for the quadratic approximation is used as the starting point for the next iteration
- This leads to Newton's recursive algorithm
<a href="https://postimg.cc/TKCRVYPG"><img src="https://i.postimg.cc/DzK8KSkW/Capture.png" width="700px" title="source: imgur.com" /><a>
<a href="https://postimg.cc/XrQCrWSB"><img src="https://i.postimg.cc/N0w7PGdD/Capture.png" width="700px" title="source: imgur.com" /><a>

<a href="https://postimg.cc/xNyYp090"><img src="https://i.postimg.cc/KjCzzzVg/Capture.png" width="700px" title="source: imgur.com" /><a>
<a href="https://postimg.cc/G8J81nQp"><img src="https://i.postimg.cc/85xhM1fL/Capture.png" width="700px" title="source: imgur.com" /><a>
<a href="https://postimg.cc/XZ9cXh8R"><img src="https://i.postimg.cc/KYH9phg8/Capture.png" width="700px" title="source: imgur.com" /><a>
<a href="https://postimg.cc/fJKc9F6X"><img src="https://i.postimg.cc/bwX3Kfg6/Capture.png" width="700px" title="source: imgur.com" /><a>
<a href="https://postimg.cc/1fs9dY4v"><img src="https://i.postimg.cc/QxWHH2DZ/Capture.png" width="700px" title="source: imgur.com" /><a>
<a href="https://postimg.cc/3d6jKpp0"><img src="https://i.postimg.cc/dVQ5jjV6/Capture.png" width="700px" title="source: imgur.com" /><a>
<a href="https://postimg.cc/9r82zXF7"><img src="https://i.postimg.cc/rwcwnzBQ/Capture.png" width="700px" title="source: imgur.com" /><a>
<a href="https://postimg.cc/KRrQxQFq"><img src="https://i.postimg.cc/sDbLP0Lr/Capture.png" width="700px" title="source: imgur.com" /><a>
<a href="https://postimg.cc/qNvFFWnX"><img src="https://i.postimg.cc/HkwCrgkK/Capture.png" width="700px" title="source: imgur.com" /><a>
<a href="https://postimg.cc/wtHQPmSK"><img src="https://i.postimg.cc/CKnrRC7K/Capture.png" width="700px" title="source: imgur.com" /><a>

# 5. THE BFGS ALGORITHM
- In 1970, an alternative update formula was suggested independently by Broyden, Fletcher, Goldfarb, and Shanno. The method is now called the BFGS algorithm, which we discuss in this section
- In numerical optimization, the Broyden–Fletcher–Goldfarb–Shanno (BFGS) algorithm is an iterative method for solving unconstrained nonlinear optimization problems

<a href="https://postimg.cc/9RH01dST"><img src="https://i.postimg.cc/4dmcfwQ8/Capture.png" width="700px" title="source: imgur.com" /><a>
<a href="https://postimg.cc/67t5vWGL"><img src="https://i.postimg.cc/Pq8L0NXc/Capture.png" width="700px" title="source: imgur.com" /><a>
<a href="https://postimg.cc/Mcgcj2qW"><img src="https://i.postimg.cc/Px519T78/Capture.png" width="700px" title="source: imgur.com" /><a>
<a href="https://postimg.cc/2qjpFVjG"><img src="https://i.postimg.cc/yNcVKZwM/Capture.png" width="700px" title="source: imgur.com" /><a>
<a href="https://postimg.cc/7J5JCHTY"><img src="https://i.postimg.cc/gjDVTrzR/Capture.png" width="700px" title="source: imgur.com" /><a>
<a href="https://postimg.cc/BLxwr3vb"><img src="https://i.postimg.cc/76c8TY83/Capture.png" width="700px" title="source: imgur.com" /><a>


[ Optimization method - Standford University ](https://web.stanford.edu/class/ee364a/lectures.html)

https://www.youtube.com/results?search_query=convex+function
