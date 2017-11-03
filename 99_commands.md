# Command Line Tasks

## Running Tasks

The distribution comes with some useful tasks that help with setting up the application and reaching as far as deploying the whole project. The tasks are implemented using GNU Make inside the Makefile.

```
$ make fix-perms
```

## Running Framework Commands

The lithium framework supports its own set of [commands](http://li3.me/docs/manual/common-tasks/console-applications.md), which we use specifically inside some modules (i.e. base_core/extensions/commands) to run certain tasks that need access to the database or the application itself.

In order to run such commands execute the following:

```
$ bin/li3.php
$ bin/li3.php help
$ bin/li3.php jobs
```
