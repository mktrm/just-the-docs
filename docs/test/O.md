---
layout: default
title: O
parent: Tests
---
# O

The `nav_order` fields of the child pages are strings.

`nav_order: "2"` always comes after `nav_order: "10"`.

Mixing numbers and strings `nav_order` fields for sibling pages
leads to Jekyll build errors.
