# Docker compose with shared mounts
Because it makes life happy.

## Prerequisites
You'll need the following flux capacitors installed:
 * [docker-toolbox](https://www.docker.com/docker-toolbox)
 * [Vagrant](https://www.vagrantup.com/downloads.html)
 * [Virtualbox](https://www.virtualbox.org/wiki/Downloads) (_or if you have it, vmware is cool too and way faster_)
 * On Windows, use [the git bash shell](https://git-for-windows.github.io/).

### Starting it up
1. Validate that vagrant, docker-machine, and docker-compose run the right commands from your command line.
2. Fork and clone this repo; navigate to it in your shell.
3. Customize `docker-compose.yml` as desired.  The default setup is a super-simple Wordpress theme editing setup.
4. Run `vagrant plugin install vagrant vagrant-host-shell`
5. Run `vagrant up`
6. Load up the Docker environment and start your services: `eval $(docker-machine env vagrant); docker-compose up`
7. Once the machines are up, just go to `172.20.254.2` in your web browser.

 > You can use the same Vagrant environment for multiple docker compositions; just set up your environmental variables with `eval $(docker-machine env vagrant)`.  The only limitation here is that you'll want to make sure your shared ports don't conflict.

### Tearing it down
1. Remove the docker-machine configuration: `docker-machine rm vagrant`
2. Destroy the vagrant box: `vagrant destroy -f`

## Under the hood

#### Terminology
 > The outmost environment (Windows, OS X, etc) is what I call the *VM host*.  On it, we run a *vagrant guest*.  The *vagrant guest* runs Docker so it's also the *docker host*.  The *docker host* runs a bunch of containers with are... *containers*.

### Notify events in the guest/docker
Things like `grunt watch`/etc will still need to be run on the *VM host*.  This is because filesystem event notifications aren't passed through the Virtualbox or VMware sharing mechanisms.

### Changing shared paths
If you want to change the paths shared with the guest, it's a two-step process: 
1. Update your Vagrantfile to share the desired *VM host* paths with the *docker host*.  
2. Update docker-compose.yml to mount the desired paths on the *docker host* to the *docker container*.  

### Changing IPs
Make sure you change IPs in both places in the Vagrantfile if you need to do so.

### Multiple providers caveat
There's a cheap hack to find the SSH key during provisioning.  If you have used multiple vagrant providers, completely remove the `.vagrant` directory from the cloned repository before bringing a new machine up.
