# Create chef-solo PHP 5.4 environment

## Description

Chef solo config files to install a php 5.4 environment that can be used with vagrant or deployed to AWS.

## Includes

* Ubuntu Precise 64
* php 5.4
* apache
* mysql
* sqlite
* git
* ntp
* vim
* zsh

## Installation

### install and vagrant

``` $ gem install vagrant```
``` $ vagrant up```

When vagrant is done, you should see the apache default ```index.html``` file in the ```www``` folder. Open ```http://localhost:8080/``` in the browser to test it.

## Run on an EC2 instance

### Requirements

you will need the key .pem file and the public DNS of your instance.

install [kinfe-solo](https://github.com/matschaffer/knife-solo)

```$ gem install knife-solo```

Create a config file (ignore the warnings)

```$ knife configure -r . --defaults```

Install ruby and vagrant on your instance

``` $ knife prepare -i ~/path/to/file.pem ubuntu@ec2-your-public-dns.compute-1.amazonaws.com```

It will also generate a json file in the ```/nodes``` folder. replace its contents with

```
  { "run_list": [
    "recipe[apt]", 
    "recipe[chef-dotdeb]", 
    "recipe[chef-dotdeb::php54]", 
    "recipe[apache2]",
    "recipe[apache2::mod_php5]",
    "recipe[apache2::mod_rewrite]",
    "recipe[php]",
    "recipe[mysql]",
    "recipe[sqlite]",
    "recipe[php]",
    "recipe[vim]",
    "recipe[zsh]",
    "recipe[cron]",
    "recipe[ntp]"
  ]}
```

Deploy the chef solo config to your instance

``` $ knife cook -i ~/path/to/file.pem ubuntu@ec2-your-public-dns.compute-1.amazonaws.com```

