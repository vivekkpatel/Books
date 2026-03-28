---
title: The Pragmatic Programmer
---

# The Pragmatic Programmer: From Journeyman to Master by Andrew Hunt and David Thomas

## Orthogonality
- In Simple terms it's decoupling of components.
- Two or more things are orthogonal if changes in one do not affect any of the others.
- For example the database code should be orthogonal to the user interface code. This means that changes to the database should not affect the user interface, and vice versa. 
- This allows for easier maintenance and scalability of the codebase, as changes can be made to one component without affecting the others.
- Benefits of Orthogonality
  - **Productivity gains **: Changes are localized, so development time and testing time are reduced.
  - **Reusability **: Orthogonal components can be reused in different contexts without modification.

## Algorithm Speed

### The Big O Notation [O()]
- Big O notation is a mathematical notation used to describe the upper bound of an algorithm's running time or space requirements in terms of the size of the input data.
- It provides a way to classify algorithms based on their performance and efficiency, allowing developers to compare different algorithms and choose the most appropriate one for a given problem.
- You can also use it for memory usage, but it's more commonly used for time.
- Common Big O Notations:
  - **O(1)**: Constant time - The algorithm's running time does not depend on the size of the input data.
  - **O(log n)**: Logarithmic time - The algorithm's running time grows logarithmically with the size of the input data.
  - **O(n)**: Linear time - The algorithm's running time grows linearly with the size of the input data.
  - **O(n log n)**: Linearithmic time - The algorithm's running time grows in proportion to n log n.
  - **O(n^2)**: Quadratic time - The algorithm's running time grows quadratically with the size of the input data.
- Downsides of Big O Notation:
  - It only provides an upper bound, so it does not give information about the actual running time of the algorithm.
  - Only highest order terms are considered, so it may not accurately reflect the performance of the algorithm for small input sizes.
  - Means O(n2/2 +3n) is same as O(n2). So one algorithm may be 1000 times faster than another but you won't know it from the notation.