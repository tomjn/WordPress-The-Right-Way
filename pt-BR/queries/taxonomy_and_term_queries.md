# Taxonomy and Term Queries

When dealing with taxonomies ( including post categories and tags ), it's safer to rely on the generic APIs rather than the legacy helper APIs. These include:

 - `get_taxonomies`
 - `get_terms`
 - `get_term_by`
 - `get_taxonomy`
 - `wp_get_object_terms`
 - `wp_set_object_terms`

It's easier to learn one set of APIs, and think of categories and tags as just another taxonomy, rather than mixing and matching older functions such as `get_category` etc.
