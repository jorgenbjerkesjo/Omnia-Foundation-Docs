Caching
============================

The Caching API makes it easy to work with object caching, either locally on one server or distributed to all servers in an Omnia server cluster (similar to Distributed Cache used in SharePoint).

You reach the Caching API through the following service

.. code-block:: c#

	OmniaApi.WorkWith(Ctx.Omnia()).Caching();
	
The API contains the following methods:

.. contents:: Sections:
  :local:
  :depth: 1

AddOrUpdateMemoryCache
#######

Adds or updates an already existing object in the memory cache

.. code-block:: c#

	void OmniaApi.WorkWith(Ctx.Omnia()).Caching().AddOrUpdateMemoryCache(string key, object value);
	
Optionally you can supply an expiration time as a **DateTimeOffset**

.. code-block:: c#

	void OmniaApi.WorkWith(Ctx.Omnia()).Caching().AddOrUpdateMemoryCache(string key, object value, DateTimeOffset expires);

 
GetFromMemoryCache
#######

Gets an object from the memory cached by suppling the key the object was stored with

.. code-block:: c#

	object OmniaApi.WorkWith(Ctx.Omnia()).Caching().GetFromMemoryCache(string key);
 
GetFromMemoryCache<T>
#######

Gets an object from the memory cached by suppling the key the object was stored with. Casts the object to the supplied type

.. code-block:: c#

	T OmniaApi.WorkWith(Ctx.Omnia()).Caching().GetFromMemoryCache<T>(string key);
 
MemoryCacheContains
#######

Checks if an object with the given key is present in the memory cache

.. code-block:: c#

	bool OmniaApi.WorkWith(Ctx.Omnia()).Caching().MemoryCacheContains(string key);
 
RemoveFromMemoryCache
#######

Deletes the object with the given key from the memory cache

.. code-block:: c#

	void OmniaApi.WorkWith(Ctx.Omnia()).Caching().RemoveFromMemoryCache(string key)

 
AddOrUpdateDistributedCache
#######

Adds or updates an already existing object in the distributed cache

.. code-block:: c#

	void OmniaApi.WorkWith(Ctx.Omnia()).Caching().AddOrUpdateDistributedCache(string key, object value, DateTimeOffset expires);
	
Optionally you can cache the data encrypted

.. code-block:: c#

	void OmniaApi.WorkWith(Ctx.Omnia()).Caching().AddOrUpdateMemoryCache(string key, object value, bool encrypted, DateTimeOffset expires);
	
You can also cache multiple objects at once by creating a **List<CachedItem>** (**CachedItem** is found in the **Omnia.Foundation.Extensibility.Core.Caching** namespace

.. code-block:: c#

	void OmniaApi.WorkWith(Ctx.Omnia()).Caching().AddOrUpdateDistributedCache(List<CachedItem> objectsToCache);
 
 
GetFromDistributedCache
#######

Gets objects from the distributed cache by supplying a list of the keys the items are stored with


.. code-block:: c#

	List<CachedItem> OmniaApi.WorkWith(Ctx.Omnia()).Caching().GetFromDistributedCache(List<string> keys);
 
GetFromDistributedCache<T>
#######

Gets an object from the distributed cache, cast to the specified type

.. code-block:: c#

	T OmniaApi.WorkWith(Ctx.Omnia()).Caching().GetFromDistributedCache<T>(string key);
 
RemoveFromDistributedCache
#######

Deletes objects from the distributed cache by supplying a list of keys

.. code-block:: c#

	void OmniaApi.WorkWith(Ctx.Omnia()).Caching().RemoveFromDistributedCache(List<string> keys);

 
