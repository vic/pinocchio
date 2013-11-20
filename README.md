# [Pinocchio] - A git puppet.

A minimalist tool for Git based server provisioning.

![Pinocchio](https://gist.github.com/vic/3494d261c68cb3b591e6/raw/acdbcdeb06606ee8a833e992e135bec6db34ecac/pinocchio.jpg)

### Why?

Server configuration and software provisioning should be damn simple.

There are lots of tools for helping you install and configure your
full software stack at your new and shiny cloud based slice. Just to name
a few, we have [Puppet], [Chef], and the [Capistrano] based [Sprinkle].
All of them trying to solve the problem of automating software installation
and configuration on many servers. Basically you write a _recipe_ file 
containing instructions on how to install or configure something, some of
these recipes require you to learn their respective mini/dsl language, 
some of them even require you to host those recipes on a service of their
own.
Sure these tools all have their own merit and can be useful when managing
lots of servers, but for me, they were sometimes just like too much complex
when I just wanted to provision a new slice to get more fun at coding a 
new experiment.

I just wanted to have some dead simple way of uploading a new configuration
file or instructions for the server on how to install something. I wanted to
deliver these just like I'm used with regular code, by using git. 
I wanted to push and see *only* the new pending instructions being executed
on my remote slices. I wanted to have all of it versioned and under control.
I wanted these changes to be incremental, something like using rails database
migrations but for provisioning my servers.

So [Pinocchio] was born.

### How it works

[Pinocchio] is just a simple git `update` hook. It works by determining which
changes it needs to execute since the last successful update. 
It does it by leveraging on git to know which *migrations* need to be run, 
and only when _*all*_ of these pending migrations have a successful exit status
the git repo is updated.

### Installation

To use [Pinocchio] on your remote server, all you need is for it to have
git installed and the pinocchio's update hook configured at a bare repo.

```shell
# install git unless you already have it on your remote server
$ ssh ubuntu@my-new-slice 'sudo apt-get install git'

# create a remote repo for pinocchio and install the update hook
$ ssh ubuntu@my-new-slice 'git init --bare ~/pinocchio'
$ scp hooks/pinocchio ubuntu@my-new-slice:~/pinocchio/hooks/update
```

### Usage

Clone [Pinocchio] into a local directory on your computer, and edit
your remotes accordingly, for our previous ubuntu example it would be:

```shell
$ git remote --set-url origin ubuntu@my-new-slice:~/pinocchio
```

Of course you can create as many remotes as servers you want to provision.

Adjust the [config](https://github.com/vic/pinocchio/blob/master/config) file
as you see fit, start writing some `migrate/` scripts and `git push`.


###### Updating your remote servers

[Pinocchio] lets you manage incremental changes on your server as
a simple git repo. So, after you've written some migration scripts
at `migrate/` or placed some configuration files under `files/` all
you need if to simply make a `git push` and the pinocchio's update
hook will take care of performing pending actions and updating files.

If all migrations run successfully, the code is updated and next time only
new migrations will be executed.

### Contributing

Feel free to contribute issues, ideas, pull-requests to [http://github.com/vic/pinocchio/issues](http://github.com/vic/pinocchio/issues) 

### Testimonials

> Just another puppet who loves using this simple tool to improve their infrastructure and development process:
>
![Infraestructur](https://gist.github.com/vic/3494d261c68cb3b591e6/raw/ab4c364f0016fc2f041b86dd842c07515d5dc758/infraestructur.png)
![Development process](https://gist.github.com/vic/3494d261c68cb3b591e6/raw/505e8b0cc8c3515810d26750da90cd9df11b0e00/development.png)


### License

MIT

[Puppet]: http://puppetlabs.com
[Chef]: http://docs.opscode.com/index.html
[Capistrano]: http://www.capistranorb.com/
[Sprinkle]: https://github.com/sprinkle-tool/sprinkle
[Pinocchio]: http://vic.github.io/pinocchio/
