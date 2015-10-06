---
layout: post
title:  "Roslyn Baby Steps: A Complete Example"
#date:   2014-08-19 12:15:41
categories:
---

Counting Dependencies
---

Let's see now a complete example.

As part of a static code analysis exercise, we want to determine the coupling in a project. We define coupling between types as: types that refer (and thus depend on) other types.

Dependencies are introduced in several ways
- inheriting from a type is the strongest of couplings
- having members (fields or properties) of a certain type
- method parameters and return values
- any types used in the implementation of the methods (incl. in getters and setters)

Approach
---

In order to find all dependencies of a type we will have to look at it and find all types on which it depends.
We might want to elimiate common types (those residing in the `System` namespace: `Object`, `int`, `string` and others).

For each type them we will have to look at:
- base type
- constructors
- fields
- properties
- methods (incl. getters and setters)
    - return type
    - parameters
    - implementation
        - local variables
        - access to other type's public interface
            - calls into methods (incl. getters and setters)
            - accessing public fields/properties

Transitive dependencies
---

We could count also transitive dependencies: types on which our dependencies depend.

