---
title: Install InvenTree
layout: page
---

# Installing InvenTree

To install a complete *development* environment for InvenTree, follow the steps presented below. A production environment will require further work as per the particular application requirements. 

A makefile in the root directory provides shortcuts for the installation process, and can also be very useful during development.

{% include alert.html title="Windows" content="If you are using the Windows operating system, it is reccomended that you use the <a href='https://docs.microsoft.com/en-us/windows/wsl/install-win10'>WSL framework</a>" %}

## Requirements

To install InvenTree you will need the following system components installed:

* python3
* python3-dev
* python3-pip (pip3)
* g++
* make

Each of these programs need to be installed (e.g. using apt or similar) before running the ``make install`` script:

```
sudo apt-get install python3 python3-dev python3-pip g++ make
```

## Virtual Environment

Installing the required Python packages inside a virtual environment allows a local install separate to the system-wide Python installation. While not strictly necessary, using a virtual environment is highly recommended as it prevents conflicts between the different Python installations.

To configure Inventree inside a virtual environment, ``cd`` into the inventree base directory and run the following commands:

``apt-get install python3-venv``

``python3 -m venv inventree-env``

``source inventree-env/bin/activate``

This will place the current shell session inside a virtual environment - the terminal should display the ``(inventree-env)`` prefix.

{% include alert.html title="Activate virtual environment" content="Remember to activate the virtual environment when starting each shell session, before running Inventree commands. This will ensure that the correct environment is being used." %}

## Installation

First, download the latest InvenTree source code:

``git clone https://github.com/inventree/inventree/``

InvenTree is a Python/Django application and relies on the pip package manager. All packages required to develop and test InvenTree can be installed via pip. Package requirements can be found in ``requirements.txt``.

To setup the InvenTree environment, *cd into the inventree directory* and run the command:

``make install``

which installs all required Python packages using pip package manager. It also creates a (default) database configuration file which needs to be edited to meet user needs before proceeding (see next step below).

Additionally, this step creates a *SECRET_KEY* file which is used for the django authentication framework. 

{% include alert.html title="Keep it secret, keep it safe" content="The SECRET_KEY file should never be shared or made public." %}

## Database Configuration

Once the required packages are installed, the database configuration must be adjusted to suit your particular needs. InvenTree provides a simple default setup which should work *out of the box* for testing and debug purposes.

As part of the previous *install* step, a configuration file (``config.yaml``) is created. The configuration file provides administrators control over various setup options without digging into the Django *settings.py* script. The default setup uses a local sqlite database with *DEBUG* mode enabled.

For further information on installation configuration, refer to the [Configuration](/docs/start/config) section.

## Initialize Database

Once install settings are correctly configured (in *config.yaml*) run the initial setup script:

``make migrate``

This performs the initial database migrations, creating the required tables, etc.

The database should now be installed!

## Create Admin Account

Create an initial superuser (administrator) account for the InvenTree instance:

``make superuser``

## Run Development Server

The InvenTree database is now setup and ready to run. A simple development server can be launched from the command line.

Run `cd InvenTree && python3 manage.py runserver 127.0.0.1:8000` to launch a development server. This will launch the InvenTree web interface at `http://127.0.0.1:8000`. For other options refer to the [django docs](https://docs.djangoproject.com/en/2.2/ref/django-admin/)

For a production install, refer to [deployment instructions](/docs/start/deploy).

## Development and Testing

Shorthand functions are provided for the development and testing process:

* ``make install`` - Install all required underlying packages using PIP
* ``make update`` - Update InvenTree installation (after database configuration)
* ``make superuser`` - Create a superuser account
* ``make migrate`` - Perform database migrations
* ``make mysql`` - Install packages required for MySQL database backend
* ``make postgresql`` - Install packages required for PostgreSQL database backend
* ``make translate`` - Compile language translation files (requires gettext system package)
* ``make backup`` - Backup database tables and media files
* ``make test`` - Run all unit tests
* ``make coverage`` - Run all unit tests and generate code coverage report
* ``make style`` - Check Python codebase against PEP coding standards (using Flake)
* ``make docreqs`` - Install the packages required to generate documentation
* ``make docs`` - Generate this documentation
