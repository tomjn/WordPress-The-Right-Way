# Routing

Common questions asked by new developers revolve around a misconception. They believe that single.php is what made WordPress load a single post, and talk about making WordPress load archive.php instead so that they can view multiple posts rather than an individual post.

That viewpoint is confusing, the truth is that such a viewpoint is completely upside down. The template does not determine the content. The content determines the template used.

This chapter will go into more depth regarding how WordPress breaks down a URL, creates a query, then figures out which template to load.

 - An explanation of how rewrite rules generate a query, which loads a template, which displays a page
 - Custom Query variables & routing
 - Adding a rewrite rule
 - Flushing rewrite rules
 - Debugging rewrite rules
 - Clashes & slugs
