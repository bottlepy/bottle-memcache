=====================
Bottle-Memcache
=====================

Memcache is a general-purpose distributed memory caching system that was
originally developed by Danga Interactive for LiveJournal, but is now used
by many other sites.

This plugin simplifies the use of memcache in your Bottle applications.
Once installed, all you have to do is to add a ``mc`` keyword argument
(configurable) to route callbacks that need a memcache connection.

Installation
===============

Install with one of the following commands::

    $ pip install bottle-memcache
    $ easy_install bottle-memcache

or download the latest version from github::

    $ git clone git://github.com/bottlepy/bottle-memcache.git
    $ cd bottle-memcache
    $ python setup.py install

Usage
===============

Once installed to an application, the plugin passes an open
:class:`memcache.Client` instance to all routes that require an
``mc`` keyword argument::

    import bottle
    import bottle.ext.memcache

    app = bottle.Bottle()
    plugin = bottle.ext.memcache.MemcachePlugin(servers=['localhost:11211'])
    app.install(plugin)

    @app.route('/show/:item')
    def show(item, mc):
        val = mc.get('item_key')
        if not val:
            mc.set('item_key', item)
            val = mc.get('item_key')
        return template('showitem', item=val)

Routes that do not expect an ``mc`` keyword argument are not affected.

Configuration
=============

The following configuration options exist for the plugin class:

* **servers**: A list of server:port items to connect to (default: ['localhost:11211',])
* **timeout**: The timeout value in seconds when connecting (default: 3)
* **keyword**: The keyword argument name that triggers the plugin (default: 'mc').

