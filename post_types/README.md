# Data

A description of the WordPress Data Model:

## Post types

Posts, pages, attachments, and menus are all different kinds of posts. You can register your own post types. Remember to flush permalinks when you change your post types or you'll get 404s. Never flush permalinks on every page load, it's expensive!

Posts also have post meta

## Comments

Comments have their own table and are capable of storing meta data. This is rarely used by developers but allows interesting things.

## Terms and Taxonomies

Tags and categories are taxonomies. Individual tags and categories are called terms. You can register your own taxonomies.

For example, a colour taxonomy. Purple would be a term in the colour taxonomy. Remember to flush permalinks if you change your taxonomy registration.

Object IDs are attached to taxonomy terms. These IDs are normally post IDs, but this is purely convention. There is nothing preventing a user or a comment taxonomy. A user taxonomy would be useful for grouping users into locations or job roles.

## Meta

Posts, comments, and users have meta

## Options

Options are stored as key value pairs in their own table. Some options have an autoload flag set and are loaded on every page load to reduce the number of queries.

## Transients

Transients are stored as options and are used to cache things temporarily

## Object Cache

By default WordPress will use in memory caching that does not persist between page loads. There are plugins available that extend this to use APC or memcache amongst others.
