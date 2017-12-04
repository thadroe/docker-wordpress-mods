### Docker Wordpress modifications for local development

Some modifications from the official Wordpress docker repo to suit my own needs developing locally. This is primarily for OSX and Windows. For linux [see this repo](https://github.com/thadroe/docker-wordpress-mods-linux).

#### The main differences between this and official Wordpress images on Docker Hub are:

- Includes wp-cli
- Support for SSL via self-signed cert

#### To start a project with docker-compose

1. In the 'php7.x-apache/docker-compose' directory copy the desired **docker-compose.yml** file to your project directory and edit the following VIRTUAL_HOST line to whatever you'd like:

~~~~
VIRTUAL_HOST: mytestsite.local,www.mytestsite.local
~~~~

Then edit your `/etc/hosts` file with:

`127.0.0.1 mytestsite.local www.mytestsite.local`

If you'd prefer to just use 'localhost', delete the VIRTUAL_HOST line

2. Run docker-compose to fire it up

`docker-compose up -d`

3. Open your install

https://mytestsite.local  
http://mytestsite.local:8000 for phpmyadmin

Accept the cert exception if using SSL/https

4. For wp-cli, you can run it with (changing 'user' for your own):

`docker-compose exec --user www-data wordpress wp`

To make that easier, you can create an alias in your ~/.bashrc file to something like, again replacing 'user' with your own):

~~~~
alias dcwp='docker-compose exec --user www-data wordpress wp'
~~~~

Then use 'dcwp' instead of 'wp'

You can run any wp-cli commands except for 'db' operations, but 'search-replace' still works.

---

**Note:** Only one docker-compose instance can be run at a time unless you change ports and adjust virtual hosts accordingly. See [https://github.com/jwilder/nginx-proxy](https://github.com/jwilder/nginx-proxy) if you'd like to explore some other possibilities of running multiple instances pumping through an nginx-proxy.

---

#### OPTIONAL - To build a local image of this for yourself

1. In the buildfiles directories of PHP and WP version you'd like to use, run the build per the example below. Make sure docker-entrypoint.sh is executable with `chmod u+x docker-entrypoint.sh`.

Example build where name includes php version and tag contains wp version:

`docker build -t="custom-wordpress-apache-php7.x:wp-4.x.x" .`

Change the name and tag to suit your preferences.

2. Check that the image exists with `docker image ls`

3. Change the wordpress image in your docker-compose.yml