# Django Caching

## Objectives

- [Setting up the cache](#Setting-up-the-cache)
- [Memcached](#Memcached)
- [DataBase Caching](#DataBase-Caching)
- [Filesystem caching](#Filesystem-caching)
- [Local-memory caching](#Local-memory-caching)
- [Dummy caching for development](#Dummy-caching-(for-development))
- [Using a custom cache backend](#Using-a-custom-cache-backend)
- [The per-site cache](#The-per-site-cache)
- [The per-view cache](#The-per-view-cache)
- [Template fragment caching](#Template-fragment-caching)
- [The low-level cache API](#The-low-level-cache-API)

Django comes with a robust cache system that lets you save dynamic pages so they don’t have to be calculated for each request.

For convenience, Django offers different levels of cache granularity:

- per site cache
- per view cache 
- template fragment cache
- low level cache api

## Setting up the cache

you have to tell it where your cached data should live, django comes with several builtin cahing backends, such as:

- Memecached
- Database
- File System
- Local Memory
- Dummy

Your cache preference goes in the CACHES setting in your settings file.

### Memcached

Memcached is a distributed memory-caching system. It is often used to speed up dynamic database-driven websites by caching data and objects in RAM to reduce the number of times an external data source must be read.

if you are interested to learn more about memcached, you can refer to following sites:

- [Memcached official site](https://memcached.org/about)
- [Memcached in wiki](https://en.wikipedia.org/wiki/Memcached)

After installing Memcached itself, you’ll need to install a Memcached binding. There are several Python Memcached bindings available

To use Memcached with Django:

- Set BACKEND to `django.core.cache.backends.memcached.<your-memcached-bindin>`
- Set LOCATION to ip:port values, or unix:path

Here are some examples:
```
# using ip:port
CACHES = {
    'default': {
        'BACKEND': 'django.core.cache.backends.memcached.PyMemcacheCache',
        'LOCATION': '127.0.0.1:11211',
    }
}

# using unix:path
CACHES = {
    'default': {
        'BACKEND': 'django.core.cache.backends.memcached.PyMemcacheCache',
        'LOCATION': 'unix:/tmp/memcached.sock',
    }
}

```

you can share a cache over multiple servers. To take advantage of this feature, include all server addresses in LOCATION, either as a semicolon or comma delimited string, or as a list.

```
CACHES = {
    'default': {
        'BACKEND': 'django.core.cache.backends.memcached.PyMemcacheCache',
        'LOCATION': [
            '172.19.26.240:11211',
            '172.19.26.242:11211',
        ]
    }
}
```
> The MemcachedCache backend is deprecated as python-memcached has some problems and seems to be unmaintained. Use PyMemcacheCache or PyLibMCCache instead.

### DataBase Caching

Django can store its cached data in your database. This works best if you’ve got a fast, well-indexed database server.

To use a database table as your cache backend:

- Set BACKEND to django.core.cache.backends.db.DatabaseCache
- Set LOCATION to tablename, the name of the database table.  

```
CACHES = {
    'default': {
        'BACKEND': 'django.core.cache.backends.db.DatabaseCache',
        'LOCATION': 'my_cache_table',
    }
}
```

Creating the cache table:

`python manage.py createcachetable`

To print the SQL that would be run, rather than run it, use the createcachetable --dry-run option.

##### Multiple databases

If you use multiple databases, you’ll  need to set up routing instructions for your database cache table.

For example, the following router would direct all cache read operations to cache_replica, and all write operations to cache_primary. The cache table will only be synchronized onto cache_primary:

```
class CacheRouter:
    """A router to control all database cache operations"""

    def db_for_read(self, model, **hints):
        "All cache read operations go to the replica"
        if model._meta.app_label == 'django_cache':
            return 'cache_replica'
        return None

    def db_for_write(self, model, **hints):
        "All cache write operations go to primary"
        if model._meta.app_label == 'django_cache':
            return 'cache_primary'
        return None

    def allow_migrate(self, db, app_label, model_name=None, **hints):
        "Only install the cache model on primary"
        if app_label == 'django_cache':
            return db == 'cache_primary'
        return None
```

### Filesystem caching

To use this backend set BACKEND to "django.core.cache.backends.filebased.FileBasedCache" and LOCATION to a suitable directory. 

For example, to store cached data in /var/tmp/django_cache, use this setting:

```
CACHES = {
    'default': {
        'BACKEND': 'django.core.cache.backends.filebased.FileBasedCache',
        'LOCATION': '/var/tmp/django_cache',
    }
}
```

### Local-memory caching

This is the default cache if another is not specified in your settings file.


### Dummy caching (for development)

it just implements the cache interface without doing anything.

This is useful if you have a production site that uses heavy-duty caching in various places but a development/test environment where you don’t want to cache and don’t want to have to change your code to special-case the latter. To activate dummy caching, set BACKEND like so:
```
    'default': {
        'BACKEND': 'django.core.cache.backends.dummy.DummyCache',
    }
}
```

### Using a custom cache backend

If you’re building your own backend, you can use the standard cache backends as reference implementations.

#### Cache arguments

These arguments are provided as additional keys in the CACHES setting. Valid arguments are as follows:

- **TIMEOUT**: The timeout cache, in seconds. defaults to 300 seconds. 'None' is equal to never expire. '0' is equal to expire immediately.
- **OPTIONS**: options passed to the cache backend. vary with each backend.
	
    - **MAX_ENTRIES**: The maximum number of entries. default is 300.
    - **CULL_FREQUENCY**: The fraction of entries that are culled when MAX_ENTRIES is reached. defaults to 3.
- **KEY_PREFIX**: string that will be add to all cache keys.

In this example, a filesystem backend is being configured with a timeout of 60 seconds, and a maximum capacity of 1000 items:

```
CACHES = {
    'default': {
        'BACKEND': 'django.core.cache.backends.filebased.FileBasedCache',
        'LOCATION': '/var/tmp/django_cache',
        'TIMEOUT': 60,
        'OPTIONS': {
            'MAX_ENTRIES': 1000
        }
    }
}
```
Here’s an example configuration for a pylibmc based backend that enables the binary protocol, SASL authentication, and the ketama behavior mode:

```
CACHES = {
    'default': {
        'BACKEND': 'django.core.cache.backends.memcached.PyLibMCCache',
        'LOCATION': '127.0.0.1:11211',
        'OPTIONS': {
            'binary': True,
            'username': 'user',
            'password': 'pass',
            'behaviors': {
                'ketama': True,
            }
        }
    }
}
```
Here’s an example configuration for a pymemcache based backend that enables client pooling (which may improve performance by keeping clients connected), treats memcache/network errors as cache misses, and sets the TCP_NODELAY flag on the connection’s socket:

```
CACHES = {
    'default': {
        'BACKEND': 'django.core.cache.backends.memcached.PyMemcacheCache',
        'LOCATION': '127.0.0.1:11211',
        'OPTIONS': {
            'no_delay': True,
            'ignore_exc': True,
            'max_pool_size': 4,
            'use_pooling': True,
        }
    }
}
```

### The per-site cache

to cache your entire site. You’ll need to add 'django.middleware.cache.UpdateCacheMiddleware' and 'django.middleware.cache.FetchFromCacheMiddleware' to your MIDDLEWARE setting, as in this example:

```
    'django.middleware.cache.UpdateCacheMiddleware',
    'django.middleware.common.CommonMiddleware',
    'django.middleware.cache.FetchFromCacheMiddleware',
]
```

Then, add the following required settings to your Django settings file:

- CACHE_MIDDLEWARE_ALIAS: The cache alias to use for storage.
- CACHE_MIDDLEWARE_SECONDS: The number of seconds each page should be cached.
- CACHE_MIDDLEWARE_KEY_PREFIX: If the cache is shared across multiple sites using the same Django installation.

there is an example to understand it better:

```
CACHE_MIDDLEWARE_KEY_PREFIX – If the cache is shared across multiple sites using the same Django installation, set this to the name of the site, or some other string that is unique to this Django instance, to prevent key collisions. Use an empty string if you don’t care.
```

FetchFromCacheMiddleware caches GET and HEAD responses with status 200, where the request and response headers allow.

UpdateCacheMiddleware sets headers in HttpResponses which affect downstream caches:

- Sets the Expires header to the current date/time plus the defined CACHE_MIDDLEWARE_SECONDS.
- Sets the Cache-Control header to give a max age for the page – again, from the CACHE_MIDDLEWARE_SECONDS setting.

If USE_I18N is set to True then the generated cache key will include the name of the active language.

### The per-view cache

to cache a view you can use cache_page decorators from django.views.decorators.cache.

```
from django.views.decorators.cache import cache_page

@cache_page(60 * 15)
def my_view(request):
    ...
```
You can pass the following arguments to cache_page:

- **first argument**: the cache timeout, in seconds.
- **cache**: spececific cache to use.(By default, default cache will be used)
- **key_prefix**: override key_prefix.

cache_page takes a single argument: the cache timeout, in seconds. 

 If multiple URLs point at the same view, each URL will be cached separately.

you can spececific cache by **cache** in cache_page decorator. by default, **default** cache will be used.

```
@cache_page(60 * 15, cache="special_cache", key_prefix="site1")
def my_view(request):
    ...
```
#### Specifying per-view cache in the URLconf

You can Specifying per-view cache in the URLconf. Here’s an example:

```
from django.views.decorators.cache import cache_page

urlpatterns = [
    path('foo/<int:code>/', cache_page(60 * 15)(my_view)),
]
```

### Template fragment caching

you can also cache template fragments using the cache template tag. To give your template access to this tag, put {% load cache %} near the top of your template.

The {% cache %} template tag caches the contents of the block.

```
{% load cache %}
{% cache 500 sidebar %}
    .. sidebar ..
{% endcache %}
```

### The low-level cache API

You can use low-level cache API to store objects in the cache with any level of granularity.

#### Basic usage

The basic interface is:

**cache.set(key, value, timeout=DEFAULT_TIMEOUT, version=None)**

`cache.set('my_key', 'hello, world!', 30)`

**cache.get(key, default=None, version=None)**

`cache.get('my_key') --> 'hello, world!'`

key should be a str, and value can be any picklable Python object.

To add a key only, and not update the value if key is exested already, you can use add()

```
>>> cache.set('add_key', 'Initial value')
>>> cache.add('add_key', 'New value')
>>> cache.get('add_key')
'Initial value'
```

**cache.get_or_set(key, default, timeout=DEFAULT_TIMEOUT, version=None)**

```
>>> cache.get('my_new_key')  # returns None
>>> cache.get_or_set('my_new_key', 'my new value', 100)
'my new value'
```
You can also pass any callable as a default value:

```
>>> import datetime
>>> cache.get_or_set('some-timestamp-key', datetime.datetime.now)
datetime.datetime(2014, 12, 11, 0, 15, 49, 457920)
```

**cache.get_many(keys, version=None)**

There’s also a get_many() interface that only hits the cache once.

```
>>> cache.set('b', 2)
>>> cache.set('c', 3)
>>> cache.get_many(['a', 'b', 'c'])
{'a': 1, 'b': 2, 'c': 3}
```
**cache.set_many(dict, timeout)**

To set multiple values more efficiently, use set_many() to pass a dictionary of key-value pairs:

```
>>> cache.set_many({'a': 1, 'b': 2, 'c': 3})
>>> cache.get_many(['a', 'b', 'c'])
{'a': 1, 'b': 2, 'c': 3}
```

**cache.delete(key, version=None)**

You can delete keys explicitly with delete() to clear the cache for a particular object:

`>>> cache.delete('a') --> True`

**cache.delete_many(keys, version=None)**

`>>> cache.delete_many(['a', 'b', 'c'])`

**cache.clear()**

if you want to delete all the keys in the cache, use cache.clear()

**cache.touch(key, timeout=DEFAULT_TIMEOUT, version=None)**

cache.touch() sets a new expiration for a key. For example, to update a key to expire 10 seconds from now:

`>>> cache.touch('a', 10) --> True`

**cache.incr(key, delta=1, version=None)** # You can guess

**cache.decr(key, delta=1, version=None)**

**cache.close()**

You can close the connection to your cache with close() if implemented by the cache backend.


### Downstream caches

coming soon. to reading this section by your self click [here](https://docs.djangoproject.com/en/3.2/topics/cache/#downstream-caches).































