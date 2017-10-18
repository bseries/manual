# Setup

First, let's get a copy of the distro, on which we will base off on our new project. For the sake of simplicity we'll call our new project _project_.
```
$ git clone git@github.com:bseries/distro.git project
```

All commands below are executed from the root of the project. So let's switch right into it.
```
$ cd project
```

To get you started quickly the distro contains a Vagrantfile. Using [vagrant](https://www.vagrantup.com/)  you can boot up a virtual machine (VM) easily. The VM already contains all necessary tools, a web server and a MariaDB database. Open a new terminal window, boot the VM and log in. The project will be mounted at `/var/www/project` inside the VM.
```
$ vagrant up
$ vagrant ssh
$ cd /var/www/project
```

The root of the project contains three important configuration files: `Envfile` holds per environment configuration settings, `Deployfile` configures the deploy target, `Hoifile` used by [Hoi](https://github.com/atelierdisko/hoi) for setting up the webserver and database. Especially the `Envfile` is very well documented.

Currently the configuration files contain placeholders, i.e. `__NAME__`. These can be filled in for you automatically as they can be derived easily from the project directory itself. They can also be replaced manually as well.
```
$ make prefill
```

As a PHP project the B-Series uses composer for managing PHP libraries. Let's install the initial round of dependencies. Let's also install some more B-Series modules.
```
$ cd app
$ composer install
$ composer require bseries/cms_post
$ composer require bseries/cms_social
$ cd -
```

Some B-modules contain an `assets` directory, which needs to be symlinked into the project's `assets` directory, so files in there can be accessed by the web server.
```
$ make link-assets
```

The B-Series is ready for globalization by default. Internally it uses data from the Unicode Consortium: the CLDR. The globalization data is too extensive to include it with the distro. The following command will download and install it locally.
```
$ make app/resources/g11n/cldr
```

From inside the VM continue to initialize the database using the following command, and create the initial users.
```
$ bin/init-db.sh
$ bin/li3.php users initial
```
