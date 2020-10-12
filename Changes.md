Changes

collection
- assign nav_parenthood, nav_top_nodes
- page
- include links nav_top_nodes

links nodes
- for each node
  - parent or active?
    - include node node
    - include_cached inactive_node node

node node
- include children node
- include links nav_children

inactive_node node
- assign nav_parenthood if needed
- include children node
- include inactive_links nav_children

inactive_links nodes
- for each node
  - include inactive_node node
