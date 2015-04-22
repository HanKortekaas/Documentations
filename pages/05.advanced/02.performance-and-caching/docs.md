---
title: Performance & Caching
taxonomy:
    category: docs
---

 One of the core features that make Kunena so compelling is just how fast it is.  This has always been a key consideration in the inherent design of Kunena and is primarily due to caching, but does include several other components.

## Performance

1. **PHP caching is critical**.  You should run a PHP **opcache** and **usercache** in order to get the best performance out of Kunena. WIth PHP 5.4 **APC cache** works well, but with PHP 5.5 and 5.6, **Zend opcache** with **APCu user cache** is slightly faster.

2. **SSD drives** can make a big difference. Most things can get cached in PHP user cache, but some are stored as files, so SSD drives can make a big impact on performance.

3. **Native hosting** will always be faster than a Virtual Machine.  VMs are a great way hosting providers can offer flexible “cloud” type environments. These add a layer of processing that will always affect performance. Kunena can still be fast on a VM (much faster than wordpress, joomla, etc), but still for optimal performance, you can't beat a native hosting option.

4. **Faster memory** is better. Because Kunena is so fast, and because many of its caching solutions use memory heavily, the speed of the memory on your server can have a big impact on performance. Kunena does not use extensive amounts of memory compared to some platforms so the amount of memory is not as important, nor does it impact performance as much, as memory type and speed.

5. **Shared hosting** is cheap and readily available, but sharing resources will always slow things down a bit. Again, Kunena can run very well on a shared server (better than other CMSes), but for ultimate speed, a dedicated server is the way to go.

6. **Multi-core processors** are better. Faster and more advanced processors will always help, but not as much as the other points.

>>> The getKunena.org runs on a single dedicated server with quad core processors, 16GB of memory and 6G SSD drives. We also run PHP 5.6 with Zend opcache and APCu user cache. The web servers do run a few other websites but not as many as you would find in a shared-hosting environment.

## Caching Options

Caching is an integral feature of Kunena that has been baked in from the start.  The caching mechanism that Kunena employs is the primary reason Kunena is as fast as it is.  That said, there are some factors to take into account.

Kunena uses the established and well-respected [Doctrine Cache](http://docs.doctrine-project.org/en/latest/reference/caching.html) library. This means that Kunena supports any caching mechanism that Doctrine Cache supports.  This means that Kunena supports:

* **Auto** _(Default)_ - Finds the best option automatically
* **File** - Stores in cache files in the `cache/` folder
* **APC** - [http://php.net/manual/en/book.apc.php](http://php.net/manual/en/book.apc.php)
* **XCache** - [http://xcache.lighttpd.net/](http://xcache.lighttpd.net/)
* **Couchbase** - [http://www.couchbase.com/](http://www.couchbase.com/)
* **Memcache** - [http://php.net/manual/en/book.memcache.php](http://php.net/manual/en/book.memcache.php)
* **Memcached** - [http://memcached.org/](http://memcached.org/)
* **MongoDB** - [http://www.mongodb.org/](http://www.mongodb.org/)
* **Redis** - [http://redis.io](http://redis.io)
* **Riak** - [http://basho.com/riak/](http://basho.com/riak/)
* **WinCache** - [http://www.iis.net/downloads/microsoft/wincache-extension](http://www.iis.net/downloads/microsoft/wincache-extension)
* **ZendDataCache** - [http://files.zend.com/help/Zend-Server/content/data_cache_component.htm](http://files.zend.com/help/Zend-Server/content/data_cache_component.htm)


By default, Kunena comes preconfigured to use the `auto` setting.  This will try **APC**, then **WinCache**, then **XCache**, and lastly **File**.  You can, of course, explicitly configure the cache in your `user/config/system.yaml` file, which could make things ever so slightly faster.

## Caching Types

There are actually **5 types** of caching happening in Kunena.  They are:

1. YAML configuration caching into PHP.
2. Core Kunena caching for page objects.
3. Twig caching of template files as PHP classes.
4. Image caching for media resources.
5. Asset caching of CSS and JQuery with pipelining.

The YAML configuration caching is not configurable, and will always compile and cache the configuration into the `/cache` folder. Image caching is also always on, and stores its processed images in the `/images` folder.

Core Kunena caching has the following configuration options as configured in your `user/config/system.yaml` file:

```
cache:
  enabled: true                        # Set to true to enable caching
  check:
    method: file                       # Method to check for updates in pages: file|folder|none
  driver: auto                         # One of: auto|file|apc|xcache|memcache|wincache
  prefix: 'g'                          # Cache prefix string (prevents cache conflicts)
```

As you can see, the options are documented in the configuration file itself.  During development sometimes it is useful to disable caching to ensure you always have the latest page edits.

`folder` cache check is going to be slightly faster than `file` but will not work reliably in all environments.  You will need to check if Kunena picks up modifications to pages on your server when using the `folder` option.  If automatic re-caching of changed pages it not critical to you, then setting this value to `none` will speed a production environment up even more. You will just need to manually [clear the cache](../Kunena-cli#clearing-Kunena-cache) after changes are made.

#### Memcache Specific Options

There are some extra configuration options that are required if you are connecting to a **memcached** server via the `memcache` driver option.  These options should go under the `cache:` group in your `user/config/system.yaml`:

```
cache:
  ...
  memcache:
    server: localhost
    port: 11211
```

>>>> Deleting a page does not clear the cache as cache clears are based on folder-modified timestamps.

<!-- -->

>>>>>> You can easily force the cache to clear by just touching/saving a configuration file.

The `cache: check: pages:` option can provide some slight performance improvements, but this will cause Kunena to not check for any page edits.  This is intended as a **Production-only** setting.

The Twig templating engine uses its own file based cache system, and there are a few options associated with it.

```
twig:
  cache: false                          # Set to true to enable twig caching
  debug: true                           # Enable Twig debug
  auto_reload: true                     # Refresh cache on changes
  autoescape: false                     # Autoescape Twig vars
```

For slight performance gains, you can disable the `debug` extension, and also disable `auto_reload` which performs a similar function to `cache: check: pages` as it will not look for changes in `.html.twig` files to trigger cache refreshes.

## Caching and Events

For the most part, [events are still fired](../../plugins/event-hooks) even when caching is enabled.  This holds true for all the events except for `onPageContentRaw`, `onPageProcessed`, `onPageContentProcessed`, `onTwigPageVariables`, and `onFolderProcessed`.  These events are run as all pages and folders are recursed and they fire on each page or folder found.  As their name implies they are only run during the **processing**, and not after the page has been cached.
