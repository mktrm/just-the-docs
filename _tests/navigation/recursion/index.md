---
title: Recursion
parent: Navigation
---
# Recursion

When different pages with children have the same title, references to parents have to be disambiguated by adding `grand_parent`, `ancestor` and/or `section` fields. 

- [Page A](a/) has a child [page with title C](ca/), and a grandchild [page with title D](dca/).
- [Page B](b/) has a child [page with title C](cb/), and a grandchild [page with title D](dcb/).
- The grandchild pages specify their parent and grandparent pages, so there is no ambiguity.



X
- S
  - T
    - U

Y
- S
  - T
    - U

## Notes

The [notes](./about) explain the background and implementation of the current proposal for recursive navigation.
