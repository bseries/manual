# Hosting

To route requests to `https://project.dev` into the VM we need to tell our developer machine about it: get the IP assigned to your VM (usually its the last one in the list of returned possible IPs), then add a new mapping to `/etc/hosts`.

```
$ vagrant ssh hostname -I
10.0.2.15 172.28.128.3
$ sudo sh -c 'echo "172.28.128.3 project.test" >> /etc/hosts'
```

Atelier Disko projects are usually served by [Hoi](https://github.com/atelierdisko/hoi), which
takes care of any needs the project has. *Inside the VM* the project can be easily loaded:
```
$ cd /var/www/project
$ sudo hoictl load
```

When Hoi isn't available inside the target environment. You will than have to take care of configuring webserver, database, cronjobs and workers yourself.
