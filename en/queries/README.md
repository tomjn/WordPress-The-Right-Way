# Queries

This chapter talks about several kinds of query. Post queries, taxonomy queries, comment queries, user queries, and general SQL queries.

Whenever possible, use the query APIs that WordPress provides, rather than directly calling the database. This allows the internal cache system to speed up your queries, and for caching plugins to help out.

Not using the WordPress APIs to perform queries means that 3rd party plugins are unable to intercept and modify requests, leading to compatibility issues, and broken or incomplete functionality.
