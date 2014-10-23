# Data

A description of the WordPress Data Model:

## Meta

Meta data is data with a key/name and a value, attached to another piece of data. Some people will know them as custom fields. Others will know them as user meta.

## Post types

Posts, pages, attachments, and menus are all different kinds of posts. You can register your own post types. Remember to flush permalinks when you change your post types or you'll get 404s. Never flush permalinks on every page load, it's expensive!

Posts also have post meta.

### Uploaded Files & Images

When you upload a file, WordPress does not reference the image or file using its URL, it uses an attachment. Attachments are posts of type `attachment`, and are referred to by their post ID. For example, when you set a featured image on a post, it stores the post ID of the image you chose in that posts meta.

As a result, you can request different sizes of an image using its attachment ID. Because it hasn't saved the URL, WordPress can generate resized versions of the original, and they can be cropped and manipulated independently.

## Comments

Comments have their own table, and are attached to a post. Comments are not a type of post however, but they are capable of storing meta data. This is rarely used by developers but allows for interesting things.

## Terms and Taxonomies

A taxonomy is a way of categorising or organising things. Items are organised using terms in that taxonomy.

For example, yellow is a term in the colour taxonomy. Big and small are both terms in the size taxonomy.

The Tags and categories that come with WordPress are both taxonomies. Individual tags and categories are called terms.

You can register your own taxonomies, but remember to flush permalinks if you change your taxonomy registration.

Taxonomy terms are tied to Object IDs, where an object ID can be any kind of data. This includes posts, users, or comments. These IDs are normally post IDs, but this is purely convention. There is nothing preventing a user or a comment taxonomy. A user taxonomy would be useful for grouping users into locations or job roles.

## Options

Options are stored as key value pairs in their own table. Some options have an autoload flag set and are loaded on every page load to reduce the number of queries.

### Transients

Transients are stored as options and are used to cache things temporarily

## Object Cache

By default WordPress will use in memory caching that does not persist between page loads. There are plugins available that extend this to use APC or memcache amongst others.
