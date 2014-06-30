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

Here are a few options for setting up a local development environment.

### Vagrant

A popular way to setup a local development evironment is [Vagrant](http://www.vagrantup.com/). Vagrant is a command line tool to easily setup  development environments. With Vagrant it is possible to create any environment you like and configure it.

For example you can define a setup for a Ubuntu environment with defined packages like GIT, PHP, and MySQL while another similiar setup uses a RedHat environment.

Some smart people already created configurations for WordPress and offers them for free. Two popular WordPress Vagrant setups are discusses below.

#### VVV

[Varying Vagrant Vagrants](https://github.com/Varying-Vagrant-Vagrants/VVV) is a Vagrant setup started by 10up, that gives you a virtual machine with a full WordPress stack, and all the tools you need to work on Core and your own websites.

Available on every operating system and actively maintained and used by prominent WordPress agencies, this is the most flexible and portable methods of getting a consistent and reliable development environment up quickly.

When installed, VVV will have a www subfolder containing all your sites for editing.

#### VagrantPress

Another WordPress development environment which is setup on Vagrant is [VagrantPress](https://github.com/chad-thompson/vagrantpress). It is in some ways not as complex as VVV but offers a great setup to start developing locally.

The main difference between VVV is that VagrantPress onyly sets up one local WordPress installation and does not alter the `hosts` file to forward a URL.

While VVV is better if you want to work on WordPress' core VagrantPress good for any development which needs just one installation.

### MAMP, WAMP, & LAMP

Most server stacks are LAMP stacks, which stands for:

 - Linux
 - Apache
 - MySQL
 - PHP

MAMP and WAMP are the Windows and OS X versions of LAMP, packaged up, and distributed as an installer. These are the most common variety of local environments. While easy to use, they have their own issues, e.g. MAMP comes as a commercial product with a trial period ( although it has a free version that comes bundled which most people use ).

### IIS

Microsoft Internet Information Services is the server software that powers Windows based servers. Variants of it come with Windows if you install the appropriate components, but knowledge of IIS setup in the WordPress community is rare. Most remote servers run an Apache or Nginx setup, and developer knowledge is geared in that direction.

IIS is not be the easiest route to take.

## Version Control

A vital part of working in teams and contributing is version control. Version control systems track changes over time and allow developers to collaborate and undo changes.

### Git

[Git is a popular decentralised system](http://git-scm.com/), if you've ever been on GitHub, you've encountered git.

### Subversion

Also known as svn, this is a centralised version control system, used for the plugin and theme repositories on WordPress.org
