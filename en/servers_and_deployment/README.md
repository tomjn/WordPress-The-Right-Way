# Servers And Deployment

## Test Your Changes

Always test changes on a local environment before copying them to your production server (see *Getting Started > Local Development Environment*.)

Make sure the development server has error-reporting turned on so you catch anything that would be invisible on the live site.

Make sure your local environment is as similar to your production server as possible (see also *Migrations*.)

- are you running the same PHP version?
- do you have the same PHP.ini settings?
- do you have the same version of MySQL?
- do you have the same Apache or Nginx version & configuration?
- do you have the same version of WordPress with the same plugins enabled?

Consider using a [staging server](https://en.wikipedia.org/wiki/Staging_site) to help with this.

If you're copying a database from development to production (or vice-versa), you'll need to change the URLs. See the [*Migrations*](https://github.com/Tarendai/WordPress-The-Right-Way/blob/master/en/servers_and_deployment/migrations.md) section in this chapter.

## Use Version Control

If you use a version control or source code management system such as [Git](http://git-scm.com/), you'll be able to roll back your changes when (not if) you make a mistake. You can 'push' your changes with a single command and updated files will first be copied to a temporary area before being deployed simultaneously. This avoids the site ever being left in a broken state if you have a slow connection.

A good server host provides SSH access, but if your hosting provider only allows SFTP access, consider using [git-ftp](https://github.com/git-ftp/git-ftp), so you can minimise the time it takes to update the site and the chance of forgetting to upload any new files. You'll still benefit from version control locally or if you're working with other developers.

Avoid using file editors on control panels like CPanel.

## Built-in Editors

WordPress has [theme and plugin editors](http://codex.wordpress.org/Editing_Files) built into the admin area.

**Avoid using them.**

- The editor is a simple HTML textarea - you get none of the code highlighting, formatting or syntax checking of an IDE or basic text editor, it might seem quicker but it's also much easier to make mistakes.
- there's no version control, you don't get a list of what you changed and the only protection is your own backups (if you remembered to make any).
- A significant error might break WordPress in such a way that you can no longer access the editor itself.

The theme/plugin editor is also a potential security risk: if someone gains access to an administrator account they can edit sensitive files on the server.

You can turn off file editing completely by adding this line to wp-config.php

    define( 'DISALLOW_FILE_EDIT', true );
