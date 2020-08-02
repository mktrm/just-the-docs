---
layout: default
title: Tests
nav_order: 999
---
# Tests

The sections below this page in the navigation illustrate how various navigation fields can be used to deal with duplicate parent titles and 

## Parents with the same title

The children of [C](./AC) and [C](./BC) distinguish which between their potential parents by specifying grandparents.

A
- C
  - D
  - E

B
- C
  - F
    - G

For deeper navigation structures, see the examples [below](#sections-using-the-same-titles-internally).

## Different navigation orders

The `nav_order` fields of the children of the same parent need to be all strings or all numbers. Children with no `nav_order` are sorted by their titles.

K
- N
- M
- L

O
- R
- Q
- P

## Sections using the same titles internally

When different parts of a website use the same page titles for navigation, reference to parents have to be distinguished by adding `section` and/or `ancestor` fields.

X
- S
  - T
    - U

Y
- S
  - T
    - U

## Pages with no title are ignored

Z

## Navigation notes

The [notes](./about) explain the background and implementation of the current proposal.
