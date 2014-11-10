# Queries

This chapter talks about several kinds of query. Post queries, taxonomy queries, comment queries, user queries, and general SQL queries.

Whenever possible, use the query APIs that WordPress provides, rather than directly calling the database. This allows the internal cache system to speed up your queries, and for caching plugins to help out.

Not using the WordPress APIs to perform queries means that 3rd party plugins are unable to intercept and modify requests, leading to compatibility issues, and broken or incomplete functionality.

## Query Limits and Performance

Some queries are more expansive than others, they simply do more work and don't scale. No amount of MySQL optimisation will fix them. For example, complex meta queries are more expensive.

One issue that most developers don't realise is scale. For example, you are listing terms in a custom taxonomy in a dropdown, and you have 5 or 10 terms. In that example the query will be fast, however, if 10,000 terms are added 6 months later, that dropdown is going to take a very long time to generate.

So always add limits to your queries, even if you don't think they're needed. Place an unrealistically high number you never expect to hit them, e.g. 100 or 1000.
