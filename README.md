# Create chef-solo PHP 5.4 environment

## Description

Chef solo config files to install a php environment that can be used with vagrant and deployed to amazon EC2 with Chef solo.

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

### install vagrant and ubuntu base box

 ``` $ gem install vagrant```

### install [cookbooks](https://github.com/opscode-cookbooks/)

```knife cookbook site install -o recipes/cookbooks apache2```
```knife cookbook site install -o recipes/cookbooks mysql```
```knife cookbook site install -o recipes/cookbooks sqlite```
```knife cookbook site install -o recipes/cookbooks php```
```knife cookbook site install -o recipes/cookbooks vim```
```knife cookbook site install -o recipes/cookbooks zsh```
```knife cookbook site install -o recipes/cookbooks cron```
```knife cookbook site install -o recipes/cookbooks ntp```
```knife cookbook site install -o recipes/cookbooks apt```

[Dotdeb repositories](https://github.com/homemade/chef-dotdeb) (for php 5.4)

```git clone git@github.com:homemade/chef-dotdeb.git recipes/cookbooks/git clone git@github.com:homemade/chef-dotdeb.git```

## Run vagrant

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

``` $ knife prepare -i ~/path/to/file.pem ubuntu@ec2-100-20-123-456.compute-1.amazonaws.com```

It will also generate a json file in the ```/nodes``` folder. replace its contents with

```...```

Run chef solo on your instance

``` $ knife cook -i ~/path/to/file.pem ubuntu@ec2-100-20-123-456.compute-1.amazonaws.com```

