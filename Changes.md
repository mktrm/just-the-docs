Changes

- don't remove nav_exclude==true nodes from nav_nodes
- sort nav_nodes before grouping as nav_parenthood
  - more efficient to sort only siblings
- remove nav/sorted
- inline nav/init in layout
- for collections, nav/page called only for the collection of the current page
- nav/page should include nav_exclude==true nodes in nav_page_children
- nav/links should ignore nav_exclude==true nodes

default:
- nav/main
- nav/crumbs
- nav/toc

nav/main:
- if collections
  - each collection
    - nav/collection collection
- else
  - nav/collection pages

nav/collection pages:
- nav/sorted pages
- assign nav_top_nodes
- assign nav_parentage
- if page in collection or no collection
  - nav/page_coord
- nav/page_links
