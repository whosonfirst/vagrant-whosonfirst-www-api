# vagrant-whosonfirst-www-api

This is a vagrant setup for running [whosonfirst-www-api](https://github.com/whosonfirst/whosonfirst-www-api) locally in an Ubuntu VM. When you finish, you should be able to run a development version of [places.mapzen.com](https://places.mapzen.com/) from [localhost:9876](http://localhost:9876/)

You will need the following requirements:

* [`git`](https://git-scm.com/)
* [VirtualBox](https://www.virtualbox.org/)
* [Vagrant](https://www.vagrantup.com/)

## Setup

Here's how to clone the repo and setup a new VM.

```
sudo mkdir -p /usr/local/mapzen
sudo chown -R $(whoami) /usr/local/mapzen
cd /usr/local/mapzen
git clone git@github.com:whosonfirst/vagrant-whosonfirst-www-api.git
cd vagrant-whosonfirst-www-api
make setup
```

## Provision your VM

Then provision and start your VM.

```
make build
```

When it finishes you'll get dropped into an SSH session.

## Set up the application

```
cd /usr/local/mapzen/whosonfirst-www-api
make setup
```

## Using a synced folder

If you use a native text editor like [Atom](https://atom.io/), you'll want to set up a folder sync to make it possible to edit files served from the VM on your host machine (i.e., edit on the Mac, run from the Linux VM). Open your `Vagrantfile` and uncomment this line.

```
config.vm.synced_folder "/usr/local/mapzen/whosonfirst-www-api", "/usr/local/mapzen/whosonfirst-www-api"
```

You'll also need to use a separate folder for your `templates_c` because of how everything inside the folder sync has its file ownership set to `vagrant`.

Do this from the guest VM. (Use `vagrant ssh` from `/usr/local/mapzen/vagrant-whosonfirst-www-api` to login.)

```
sudo mkdir -p /usr/local/mapzen/flamework/templates_c
sudo chown www-data:www-data /usr/local/mapzen/flamework/templates_c
```

Then add this to your `/usr/local/mapzen/whosonfirst-www-api/www/include/config_local.php`:

```
$GLOBALS['cfg']['smarty_compile_dir'] = '/usr/local/mapzen/flamework/templates_c';
```

## Setting the abs_root_url

You may also need to add this to `/usr/local/mapzen/whosonfirst-www-api/www/include/config_local.php`:

```
$GLOBALS['cfg']['abs_root_url'] = 'https://localhost:4444';
```
