Readme
======

This Chef repository aims at being the easiest way set up and configure your own Rails server
to host one or more Ruby on Rails applications using best
practices from the community. **These recipes do not require a Chef server**

The configuration is heavily inspired by blog posts and chef recipes
from [37signals](http://37signals.com) and the
[Opscode Community Cookbooks](http://community.opscode.com). It takes
care of automatic installation and configuration of the following software
on a single server or multiple servers:

* Nginx as webserver
* Unicorn as the Rails application server (with rolling restarts)
* App deployment with Capistrano

The Chef recipes in this repository are meant to set up servers with a bare
**Ubuntu 12.04(.x) LTS (Precise Pangolin)** installation and that you have root
or sudo access.

## Getting started

### Server prerequisites

Your server has:

* Ubuntu 12.04 LTS or a later minor version. The latest is Ubuntu 12.04.3 LTS.

### Installation

Clone the repository onto your own workstation.

```sh
git clone git://github.com/intercity/chef-repo.git chef_repo
```

Run bundle to install chef and knife-solo:

```sh
bundle install
```

### Setting up the server

Prepare the server with `knife solo`. This installs Chef.

```sh
bundle exec knife solo prepare <your user>@<your host/ip>
```

This will create `nodes/<your host/ip>.json`. Copy the contents from `nodes/sample_host.json` into
your host json file.

In the file, replace the samples between `< >` with the values for your server and applications.

Then, install everything to run Rails apps on your server with the next command. You might need to enter your password a couple of times.

```sh
bundle exec knife solo cook <your user>@<your host/ip>
```

### Deploying your applications

The two commands in the previous section prepare your apps to be deployed with
Capistrano. The folder structure for each app on your server looks like:

```
/u/apps/your_app
  current/
  releases/
  shared/
    config/
      database.yml
      unicorn.rb
    pids/
    log/
    sockets/
```

To deploy your application:

Add the `intercity` gem to your `Gemfile`:

```ruby
# Your Gemfile

# other gems..

gem 'intercity'
```

Run

```sh
capify .
```

to generate the Capistrano configuration files.

Uncomment the

```
load 'deploy/assets'
```

line in the generated `Capfile` so it looks like this:

```ruby
load 'deploy'
load 'deploy/assets'
load 'config/deploy'
```

Open `config/deploy.rb` and set the `application` and `repository` settings.

Run

```sh
cap deploy:check
```

to see if everything is set up. And then

```sh
cap deploy
```

to deploy your application!

## Getting help

The following steps will let you **set up or test your own Rails infrastructure
in 5 - 10 minutes**. If something doesn't work or you need more instructions:

**Please!** [Open an issue](https://github.com/firmhouse/locomotive-chef-repo/issues) or email [michiel@firmhouse.com](mailto:michiel@firmhouse.com).

## Resources and original authors

* Most of the cookbooks that are used in this repository are installed from the [Opscode Community Cookbooks](http://community.opscode.com).
* The `rails` and `bluepill` configuration is based off the cookbooks by [jsierles](https://github.com/jsierles) at https://github.com/jsierles/chef_cookbooks
