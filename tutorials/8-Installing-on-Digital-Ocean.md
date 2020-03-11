# Installing on Digital Ocean

This tutorial will show you how to setup your grocery store on Digital Ocean. Prior to this tutorial you should have:

- an account on a Digital Ocean server
- a domain name
- configuration of your domain name so that `mydomain.com` points to the IP address of your server
- configuration of your domain name so that `*.mydomain.com` likewise points to the IP address of your server.
- the ability to clone repositories from GitHub onto your Digital Ocean server

## Setup your website

We need to configure your web server, nginx, so that it has a new server block for this lab. Let's assume it is called `motherearth.mydomain.com`. Substitute your own domain as needed.

First, make a directory, where you will store the files, for example:

```
sudo mkdir /var/www/motherearth.mydomain.com
sudo chown [username] /var/www/motherearth.mydomain.com
```

Substitute your username above. This will both create the directory and give you
ownership over it.

Next, navigate to the directory where nginx sites are configured:

```
cd /etc/nginx/sites-available
```

Create a new file there:

```
sudo touch motherearth.mydomain.com
```

Edit this file with the editor of your choice. Be sure to use sudo. Put the following there:

```
server {
  listen 80;
  server_name motherearth.mydomain.com;
  root /var/www/motherearth.mydomain.com;
  index index.html;
  default_type "text/html";
  location / {
    # Serve static files from the root location above.
    try_files $uri $uri/ /index.html;
  }
}
```

This will setup nginx so that any requests to `motherearth.mydomain.com` go to `/var/www/motherearth.mydomain.com`.

The next step is to link this into the `sites-enabled` directory:

```
cd ../sites-enabled
sudo ln -s ../sites-available/motherearth.mydomain.com .
```

You should be able to see that this is working properly:

```
ls -al
```

You will see something like this:

```
lrwxrwxrwx 1 root root   34 Mar 14 01:20 motherearth.mydomain.com -> ../sites-available/motherearth.mydomain.com
```

This shows that the file is linked correctly. You can also try using `cat` to view the file and make sure it looks right.

The final step is to reload nginx so that this configuration takes effect:

```
sudo nginx -s reload
```

If nginx fails to reload properly, then most likely you have a syntax error in the configuration for a host, or your soft link (from `ln -s`) is incorrect. You can view the exact error with:

```
cat /var/log/nginx/error.log
```

## Install nvm

Go back to your home directory:

```
cd
```

Navigate to [nvm](https://github.com/creationix/nvm) and find the curl command used to install nvm.

For example:

```
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.35.2/install.sh | bash
```

Then install the stable version of node and npm and use it:

```
source ~/.bashrc
nvm install stable
nvm use stable
```

## Clone your code

Clone your repository into your **home directory**. For example:

```
cd ~/
git clone [PathToYourRepositoryOnGitHub]
cd [RepoDirectory]
```

Note, you should *not* have `node_modules` in your repository. This is huge and
wasteful, because we can easily reinstall all the packages you need on your server.

## Install Packages

Now install the packages you will need:

```
npm install
```

This will find all the packages in package-lock.json and install them into this directory.

## Build your code

Build your code by using:

```
npm run build
```

This will create an entire copy of your website front end in the `dist/` directory.

## Setup your website

Now copy your public files to `/var/www`. For example:

```
cp -rp dist/* /var/www/motherearth.mydomain.com/
```

## Testing

Everything should be setup. You should be able to browse to your site, e.g `motherearth.mydomain.com` and have the site work.

## Debugging

If your site is not working, you should (1) look at the JavaScript console, (2) look at the Network tab in Developer Tools, (3) look at Vue state in Vue Developer Tools.
