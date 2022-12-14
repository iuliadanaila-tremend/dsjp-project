# Digital Skills and Jobs Platform sample install

<p>A base project for DSJP project.</p>
<p>This is a Composer-based installer for the DSJP profile distribution.

This will create a DSJP sample site and install all the project's dependencies.
</p>

- [Subsite](#subsite)
  * [1. Development](#1-development)
    + [1.1 Prerequisites](#11-prerequisites)
    + [1.2 Configuring the project](#12-configuring-the-project)
    + [1.3 Setting up the environment](#13-setting-up-the-environment)
      - [1.3.1 Make your life easier with aliases](#131-make-your-life-easier-with-aliases)
    + [1.4 Installing the project](#14-installing-the-project)
    + [1.5 Testing the project](#15-testing-the-project)
    + [1.6 Updating composer.lock](#110-updating-composerlock)

<small><i><a href='http://ecotrust-canada.github.io/markdown-toc/'>Table of contents generated with markdown-toc</a></i></small>

## 1. Development

### 1.1 Prerequisites

You need to have the following software installed on your local development
environment: [Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git),
[Docker](https://docs.docker.com/install/) and [Docker Compose](https://docs.docker.com/compose/install/)

### 1.2 Configuring the project

The project ships with default configuration in the `runner.yml.dist` file. This
file is configured to run the website on the provided docker containers. If you
are happy using those, you can skip directly to the [installing the project](#14-installing-the-project)
section. You can customize the default configuration by copying `runner.yml.dist`
to `runner.yml` and changing for example the connection details for your
database server and selenium server.

### 1.3 Setting up the environment

By default, docker-compose reads two files, a `docker-compose.yml` and an
optional `docker-compose.override.yml` file. By convention, the `docker-compose.yml`
contains your base configuration and it is committed to the repository. This
file contains a webserver, a mysql server and a selenium server. It very closely
matches the environment the website is deployed on.

The override file, as its name implies, can contain configuration overrides for
existing services, or it can add entirely new services. This file is never
committed to the repository.

#### 1.3.1 Make your life easier with aliases

You can shorten the commands listed below by setting an alias in your `.bashrc`
file:
```bash
alias dcup="dc up -d"
alias dcweb="dc exec web "
```

#### 1.3.1 Make your life easier with ahoy

You can shorten the commands listed below by using ahoy cli. Install it an check
available commands by running
```bash
ahoy
```

### 1.4 Installing the project

```bash
Request access to GitHub - iuliadanaila-tremend/dsjp-project

Add SSH key to your Github account.

Steps

Clone repository
git clone git@github.com:iuliadanaila-tremend/dsjp-project.git

Run the installation

docker-compose up -d

docker-compose exec web composer install

Make sure the profile is installed:

docker-compose exec web composer require digit/dsjp --update-with-dependencies

docker-compose exec web ./vendor/bin/run toolkit:build-dev

docker-compose exec web ./vendor/bin/drush site-install dsjp_profile

Start docker

# Using default docker-compose.yml
docker-compose up -d

# (optional) Using a custom docker-compose.override.yml
docker-compose -f docker-compose.override.yml up -d

Done! Visit  http://test:8080/web
```

Using default configuration your Drupal site will be available locally at:
- [http://127.0.0.1:8080](http://127.0.0.1:8080)
  - does not support EU Login
  - no http auth by default

**NOTE:** If Cloud9 is used for the project development, when
setting up a project there it will be available at either:
- [https://|your-c9-username|.c9.fpfis.tech.ec.europa.eu/web](https://|your-c9-username|.c9.fpfis.tech.ec.europa.eu/web)
  - supports EU Login
  - http auth by default (request credentials with a teammember)
- [https://|aws-machine-id|.vfs.cloud9.eu-west-1.amazonaws.com/web](https://|aws-machine-id|.vfs.cloud9.eu-west-1.amazonaws.com/web)
  - does not support EU Login
  - protected by C9 session


### 1.5 Testing the project
```bash
# Run coding standard checks
docker-compose exec web ./vendor/bin/run toolkit:test-phpcs
# Run behat tests on a clean installation.
docker-compose exec web ./vendor/bin/run toolkit:test-behat
# Run behat tests on a clone installation.
docker-compose exec web ./vendor/bin/run toolkit:test-behat -D "behat.tags=@clone"
```

### 1.6 Updating composer.lock
When having a conflict on the composer.lock file it is best to solve the conflict
manually and then update the lock file.

```bash
docker-compose exec web composer update --lock
```

### 1.7 Tips for local development
#### Config Split - Not yet used
Add the following line into settings.override.php
```bash
$config['config_split.config_split.dev']['status'] = TRUE;
```

#### Mailhog
There is a docker container, Mailhog for viewing and debugging emails on local environment. In your browser
visit http://web:8025/. You can change the port in docker-compose.override.yml

#### Solr
Run the following command to create solr core
```bash
docker-compose exec solr /bin/bash
make create core=dsjp -f /usr/local/bin/actions.mk
```
Go into admin and reindex content.
