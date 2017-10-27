# Setup

First, let's get a copy of the distro, off which we will base our new project. For the sake of simplicity we'll call our new project _example_. After cloning switch right into the directory.
```
$ git clone git@github.com:bseries/distro.git example
$ cd example
```

The distro comes with a Vagrantfile, used by the [vagrant](https://www.vagrantup.com/) program to boot up a virtual machine (VM) for development purposes. The VM already contains all necessary tools, a web server and a MariaDB database.  

Let's first boot the VM and establish a route so requests to `project.test` will go right into the VM.
```
$ vagrant up
$ vagrant ssh "hostname -I"
10.0.2.15 172.28.128.3
$ sudo sh -c 'echo "172.28.128.3 project.test" >> /etc/hosts'
```

The `hostname -I` often returns multiple IP addresses, use the one that looks the most legit to you (usually its the last one).

Our development machine is now running nicely, let's continue setting up the project. The outside project has been mounted at `/var/www/project` inside the VM.
```
$ vagrant ssh
$ cd /var/www/project
```

The root of the project contains three important configuration files: `Envfile` holds per environment configuration settings, `Deployfile` configures the deploy target, `Hoifile` used by [Hoi](https://github.com/atelierdisko/hoi) for setting up the webserver and database. The `Envfile` is well documented.

Currently the configuration files contain placeholders, i.e. `__NAME__`. These can be filled in for you automatically as they can be derived easily from the project directory itself. They can also be replaced manually as well.
```
$ make prefill
```

[Hoi](https://github.com/atelierdisko/hoi) is a host management program that orchestrates other services, so projects can be hosted with the execution of just one command. It automates setting up
several aspects of a project i.e. SSL certificates, databases, cron jobs and HTTP auth for staging areas.

Hoi will generate all necessary web server configuration files and an empty database for us.
```
$ sudo hoictl load
project successfully loaded :)
```

As a PHP project the B-Series uses [composer](https://getcomposer.org/) for managing PHP libraries. Let's install the initial round of dependencies as well as some more B-Series modules.
```
$ cd app
$ composer install
$ composer require bseries/cms_post
$ composer require bseries/cms_social
$ cd -
```

Some B-modules contain an `assets` directory, which need to be symlinked into the project's `assets` directory, so files in there can be accessed by the web server. This needs to happen after installing the composer dependencies above and everytime you have added a new B-module.
```
$ make link-assets
```

The B-Series is ready for globalization by default. Internally it uses data from the Unicode Consortium: the CLDR. The globalization data is too extensive to include it with the distro. The following command will download and install it locally.
```
$ make app/resources/g11n/cldr
```

One of the last steps is to initialize the database create the initial users for the administration panel.
```
$ bin/init-db.sh
Importing app/libraries/base_core/data/schema.sql into database project
Importing app/libraries/base_media/data/schema.sql into database project

$ bin/li3.php users create Admin admin@example.com secret admin
Created user with:
name     : Admin
email    : admin@example.com
password : secret
role     : admin
```

![admin panel login screenshot](http://b-series.org/assets/v:1+9cc014f/app/img/login.png)

**The application is now ready for development**: https://project.test

**Login to the administration panel under**: https://project.test/admin/session
