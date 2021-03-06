---
title: Deploy InvenTree
layout: page
---

# Deploying InvenTree

The development server provided by the Django ecosystem may be fine for a testing environment or small contained setups. However special consideration must be given when deploying InvenTree in a real-world environment.

Django apps provide multiple deployment methods - see the [Django documentation](https://docs.djangoproject.com/en/2.2/howto/deployment/).

There are also numerous online tutorials describing how to deploy a Django application either locally or on an online platform.

# Gunicorn

Following is a simple tutorial on serving InvenTree using [Gunicorn](https://gunicorn.org/). Gunicorn is a Python WSGI server which provides a multi-worker server which is much better suited to handling multiple simultaneous requests. 

## Install Gunicorn

Gunicorn can be installed using PIP:

``pip3 install gunicorn``

## Configure Static Directories

Directories for storing *media* files and *static* files should be specified in the ``config.yaml`` configuration file. These directories are the ``MEDIA_ROOT`` and ``STATIC_ROOT`` paths required by the Django app. Ensure that both of these directories are correctly configured for your setup.

## Collect Static Files

The required static files must be collected into the specified ``STATIC_ROOT`` directory:

`make static`

This command collects all of the required static files (including script and css files) into the specified directory ready to be served.

## Configure Gunicorn

The Gunicorn server can be configured with a simple configuration file (e.g. python script). An example configuration file is provided in ``InvenTree/gunicorn.conf.py``

```python
{% include gunicorn.conf.py %}
```

This file can be used to configure the Gunicorn server to match particular requirements.

## Run Gunicorn

Run ``cd InvenTree && gunicorn -c gunicorn.conf.py InvenTree.wsgi``
