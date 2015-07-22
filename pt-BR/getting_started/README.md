# Getting Started

## Basic PHP

It's assumed that you have a basic knowledge of PHP. This will include a knowledge of:

 - [functions](http://www.php.net/manual/en/language.functions.php)
 - [arrays](http://www.php.net/manual/en/language.types.array.php)
 - [variables](http://www.php.net/manual/en/language.variables.php)
 - [loops and conditionals](http://www.php.net/manual/en/language.control-structures.php)
 - [classes and objects](http://www.php.net/manual/en/language.oop5.php)
 - [class inheritance](http://www.php.net/manual/en/language.oop5.inheritance.php)
 - [polymorphism](http://code.tutsplus.com/tutorials/understanding-and-applying-polymorphism-in-php--net-14362)
 - [POST](http://www.php.net/manual/en/reserved.variables.post.php) and [GET](http://www.php.net/manual/en/reserved.variables.get.php)
 - [variable scope](http://www.php.net/manual/en/language.variables.scope.php)

If you don't have a good grasp of those concepts, you should make sure you have a firm understanding before continuing.

It's also assumed you have a code editor that has PHP syntax highlighting, although these will be beneficial:

 - Auto Indenting
 - Auto-completion
 - Brace matching
 - Syntax checking

## Local Development Environments

It's important to have a local development environment. Gone are the old days of changing a PHP file then updating it on the live server and hoping for the best.

With a local environment, you can work faster, no more uploading and downloading files, being at the mercy of a dodgy internet connection, or waiting for pages to load from the open web. With a local server stack you can work on a train in a tunnel with no wifi or phone signal, and test your work before deploying it to the live server.

Here are a few options for setting up a local development environment. They fall into two categories:

 - Virtual Machines
 - Native Server Stacks

The first type of environment usually involves projects such as Vagrant, and gives you a standardised consistent virtual machine to work with.

The second, installs the server software directly into your operating system. There are various tools that make this easy, but your environment will be unique and more difficult to debug. These are sometimes called LAMP stacks, which stands for Linux Apache MySQL PHP.

### IIS

Microsoft Internet Information Services is the server software that powers Windows based servers. Variants of it come with Windows if you install the appropriate components, but knowledge of IIS setup in the WordPress community is rare. Most remote servers run an Apache or Nginx setup, and developer knowledge is geared in that direction.

IIS is not the easiest route to take.

## Version Control

A vital part of working in teams and contributing is version control. Version control systems track changes over time and allow developers to collaborate and undo changes.

### Git

Created by Linus Torvalds the creator of Linux, [Git is a popular decentralised system](http://git-scm.com/), if you've ever been on GitHub, you've encountered git.

### Subversion

Also known as svn, this is a centralised version control system, used for the plugin and theme repositories on WordPress.org
