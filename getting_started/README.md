# Getting Started

## Basic PHP

It's assumed that you have a basic knowledge of PHP. This will include a knowledge of:

 - functions
 - arrays
 - variables
 - loops and conditionals
 - classes and objects
 - class inheritance
 - polymorphism
 - POST and GET
 - variable scope

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

Another WordPress devlepment environment which is setup on Vagrant is [VagrantPress](https://github.com/chad-thompson/vagrantpress). It is in some ways not as complex as VVV but offers a great setup to start developing locally.

The main difference between VVV is that VagrantPress onyly sets up one local WordPress installation and does not alter the `hosts` file to forward a URL.

While VVV is better if you want to work on WordPress' core VagrantPress good for any development which needs just one installation.

### MAMP, WAMP, & LAMP

### IIS

Microsoft Internet Information Services is the server software that powers Windows based servers. Variants of it come with Windows if you install the appropriate components, but knowledge of IIS setup in the WordPress community is rare. Most remote servers run an Apache or Nginx setup, and developer knowledge is geared in that direction.

IIS is not be the easiest route to take.

## Version Control

### Git

### Subversion

Also known as svn, this is a centralised version control system, used for the plugin and theme repositories on WordPress.org
