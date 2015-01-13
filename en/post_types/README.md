# Data

A description of the WordPress Data Model:

## Meta

Meta data is data with a key/name and a value, attached to another piece of data. Some people will know them as custom fields. Others will know them as user meta.

## Post types

Posts, pages, attachments, and menus are all different kinds of posts. You can register your own post types. Remember to flush permalinks (manually: Dashboard > Settings > Permalinks > Save Changes or programmatically via [flush_rewrite_rules](http://codex.wordpress.org/Function_Reference/flush_rewrite_rules)) when you change your post types or you'll get 404s. Never flush permalinks on every page load, it's expensive!

Posts also have post meta.

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

WordPress generates and saves resized versions of the original at the time of upload, for better performance. They can be cropped and manipulated independently and the dimensions and filenames are stored in the attachment postmeta.

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
