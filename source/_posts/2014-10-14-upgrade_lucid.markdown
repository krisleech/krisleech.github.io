# Upgrading Lucid LTS (10.04) to Precise LTS (12.04) to Trusty LTS (14.04)

First backup your server as a new restorable image.

## Get the latest packages for Lucid:

```
sudo apt-get update
sudo apt-get upgrade
```

Change release-upgrades `prompt` to `lts`:

```
sudo vim /etc/update-manager/release-upgrades
```

Change `Prompt` to `lts`.

The above was needed otherwise I got a "Failed Upgrade tool signature" error.

Start a `tmux` session (in case your network goes down mid-upgrade):

```
tmux
```

## And start the actual upgrade:

```
sudo apt-get install update-manager-core
sudo do-release-upgrade
```

You will get asked if you want to overwrite configuration files which have been changed since install. I choose "no" for all Apache config files and `/etc/sudoers` and "yes" for everything else.

## Unmet dependencies

If you get an errors at the end check for unmet dependencies:

```
sudo dpkg --audit
```

Most unmet dependencies can be fixed with:

```
sudo apt-get -f install
```

This did not work for all unmet dependencies and I had to remove the remaining half installed package (in my case `git`):

```
sudo dpkg --remove <package>
sudo apt-get install <package>
```

## Finally

`exit` the tmux session and `sudo reboot now`.

If everything is okay you can repeat the process to upgrade Trusty 14.0.4 LTS, if desired. But take **another** backup image first!

After this running `sudo do-release-upgrade` you should get "Now new release found".

## Fixing Apache

I choose to keep my existing Apache config files as a result a few things needed fixing:

* Remove `Include http.conf` and `Include conf.d/*` in `apache.conf` as these no longer exist
* Replace `LockFile` with `Mutex file:${APACHE_LOCK_DIR} default` in `apache.conf`

* Reinstall Passenger
  * Install Apache dev headers: `sudo apt-get install apache2-threaded-dev libapr1-dev libaprutil1-dev`
  * Comment out existing passenger lines in `apache.conf` and `site-enabled/*.conf` (because passenger will not install until Apache is restartable)
  * `gem install passenger`
  * `rvmsudo passenger-install-apache2-module`
  * Grab a coffee...
  * Take note of the post install instructions and paste in the relevant lines in to `apache2.conf`
  * Uncomment the passenger related lines in `site-enabled/*.conf`
  * `sudo apachecrl restart`

Pat yourself on the back, no need for a full upgrade for many years.