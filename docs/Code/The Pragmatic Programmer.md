---
title: The Pragmatic Programmer
---

# The Pragmatic Programmer: From Journeyman to Master by Andrew Hunt and David Thomas

## A Pragamatic Approach

### Orthogonality
- In Simple terms it's decoupling of components.
- Two or more things are orthogonal if changes in one do not affect any of the others.
- For example the database code should be orthogonal to the user interface code. This means that changes to the database should not affect the user interface, and vice versa. 
- This allows for easier maintenance and scalability of the codebase, as changes can be made to one component without affecting the others.
- Benefits of Orthogonality
  - **Productivity gains **: Changes are localized, so development time and testing time are reduced.
  - **Reusability **: Orthogonal components can be reused in different contexts without modification.