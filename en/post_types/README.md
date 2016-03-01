# Data

There are multiple data types in WordPress, but the basic data types stored in the database are:

 - Post
 - Comments
 - Terms
 - User
 - Blogs
 - Network
 - Links
 - Options
 - Site Options

Some of these data types can have additional data attached to them in the form of meta data, some of them have types and statuses.

There are also roles and capabilities, which are not considered content and apply exclusively to users.

## Meta

Meta data is data with a key/name and a value, attached to another piece of data. Some people will know them as custom fields. Others will know them as user meta.

Post Meta ( or Custom fields ) are normally shown in the post edit screen, but if the post meta's key/name begins with an underscore, that field is hidden. This way features such as featured images can be built with their own user interfaces.

## Post types

Posts, pages, attachments, and menus are all different kinds of posts. You can also register your own post types, and these are referred to as custom post types. Remember to flush permalinks (manually: Dashboard > Settings > Permalinks > Save Changes or programmatically via [flush_rewrite_rules](http://codex.wordpress.org/Function_Reference/flush_rewrite_rules)) if you modify or add a post type. If you do not do this, some links will generate 404 errors.

*Warning: Developers sometimes attempt to work around the rewrite rules by flushing rewrite rules on the `init` action using the `flush_rewrite_rules`, but this is a mistake. It can lead to unexpected behaviours, and has a large negative performance impact. Rewrite rules are expensive to build.

### Default Post Types

The default post types are as follows:

 - Post (`post`)
 - Page (`page`)
 - Attachment (`attachment`)
 - Revision (`revision`)
 - Navigation menu (`nav_menu_item`)

### Menus

Menu items are stored as `nav_menu_item` posts, however, the menu itself is a term in a custom taxonomy that contains `nav_menu_item` posts.

### Revisions

A post of type `revision`, revisions are historical copies of posts, and are tied to their original post via the post_parent field. To get a posts revisions, [grab all its children](http://codex.wordpress.org/Function_Reference/get_children) of type `revision`. If a database is growing very large or a particular post/page is frequently/automatically edited, you can [limit the number of revisions stored](http://codex.wordpress.org/Revision_Management#Revision_Options).

### Uploaded Files & Images

When you upload a file, WordPress does not reference the image or file using its URL, it uses an attachment. Attachments are posts of type `attachment`, and are referred to by their post ID. For example, when you set a featured image on a post, it stores your chosen image's post ID in that post's meta (`_thumbnail_id`).

If a file is uploaded whilst editing a post (rather than in the Media Library itself), it's `post_parent` field is set to that of the post.

WordPress generates and saves resized versions of the original at the time of upload for better performance. They can be cropped and manipulated independently and the dimensions and filenames are stored in the attachment postmeta.

The default image sizes are configured in Dashboard > Settings > Media. If you need more, you can use [add_image_size](http://codex.wordpress.org/Function_Reference/add_image_size) and also control cropping. You can request a specific size of image using the attachment ID and the image size name (e.g. 'medium', 'large'.)

The [Regenerate Thumbnails](https://wordpress.org/plugins/regenerate-thumbnails/) plugin is useful if you change sizes at a later date.

## Comments

Comments have their own table, and are attached to a post. Comments are not a type of post however, but they are capable of storing meta data. This is rarely used by developers but allows for interesting things.

## Terms and Taxonomies

A taxonomy is a way of categorising or organising things. Items are organised using terms in that taxonomy.

For example, yellow is a term in the colour taxonomy. Big and small are both terms in the size taxonomy.

The Tags and categories that come with WordPress are both taxonomies. Individual tags and categories are called terms.

You can [register your own taxonomies](https://codex.wordpress.org/Taxonomies#Registering_a_taxonomy), but remember to flush permalinks (see *Post Types* above) if you make changes.

Taxonomy terms are tied to Object IDs, where an object ID can be any kind of data. This includes posts, users, or comments. These IDs are normally post IDs, but this is purely convention. There is nothing preventing a user or a comment taxonomy. A user taxonomy would be useful for grouping users into locations or job roles.

## Options

Options are stored as key value pairs in their own table. Some options have an autoload flag set and are loaded on every page load to reduce the number of queries.

### Transients

Transients are stored as options and are used to cache things temporarily

## Object Cache

By default WordPress will use in memory caching that does not persist between page loads. There are [plugins available](http://codex.wordpress.org/Class_Reference/WP_Object_Cache#Persistent_Cache_Plugins) that extend this to use APC or Memcache amongst others.

## Data Overview

Here's a table showing the full spectrum of data types in WordPress that are stored in the database: 

|                  |Post                   |Comments             |Term|User|Blogs|Network|Links|Options|Site Options
|------------------|-----------------------|---------------------|--------|----|----|-----|-------|-----|----|----
|Description       |Content, e.g. articles |Commentary on a post |A type of objects, for classifying |Users and Authors      |A website |A collection of websites |Links to websites|key value pairs|Network-wide key value pairs
|Supported         |Yes                    |Yes                  |Yes |Yes |Yes |Multisite |Deprecated|Yes|Yes
|Meta              |Custom fields          |Comment meta         |Planned |User Meta |Options |Site Options |No|No|No
|Meta Access       |`get_post_meta`        |`get_comment_meta`   |Planned |get_user_meta |get_option |get_site_option |n/a|n/a|n/a
|Type              |Post type              |Comment type         |Taxonomy |Roles and Capabilities |No |subdomain or subdir |link_category taxonomy|n/a|n/a
|Type registration |`register_post_type`   |defined on use       |`register_taxonomy` |`add_role add_cap` |n/a |wp-config.php DEFINE |n/a|n/a|n/a
|Taxonomy UI?      |Yes                    |No                   |Yes |No |No |No |only link_category|n/a|n/a
|Default Types     |post page attachment nav_menu nav_menu_item |none pingback trackback |cat tag |admin editor author contributor subscriber |n/a |SUBDOMAIN_INSTALL true/false |n/a|n/a|n/a
|Query Class       |`WP_Query`             |`WP_Comment_Query`   |`get_terms` |`WP_User_Query` |`wp_get_sites` |`wp_get_sites` |`get_links`|`get_option`|`get_site_option`
|Has Archives      |Yes                    |No |Per blog |Yes |No |No |No|No|No
|Has Widget        |Recent Posts           |Recent Comments |Categories and Tags |No |No |No |Bookmarks|No|No
|Data Availability |Per blog               |Per blog |Per blog |Per install |Per network |Per install |Per blog|Per blog|Per network
|Set current       |`setup_postdata`       |n/a|n/a |n/a |switch_to_blog |no native function |n/a|n/a|n/a
|Database Table    |`wp_posts`             |`wp_comments` |`wp_terms` `wp_term_relationships` `wp_term_taxonomy` |`wp_users` |`wp_blogs` |`wp_site` |`wp_links`|`wp_options`|`wp_site_options`
|Meta Table        |`wp_postmeta`          |`wp_commentmeta` |Planned |`wp_usermeta` |`wp_options` |`wp_sitemeta` |n/a|n/a|n/a
