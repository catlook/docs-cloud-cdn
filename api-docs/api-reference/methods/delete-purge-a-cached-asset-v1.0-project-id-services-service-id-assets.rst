.. _cdn-purge-a-cached-asset:

Purge a cached asset
~~~~~~~~~~~~~~~~~~~~

.. code::

    DELETE /v1.0/{project_id}/services/{service_id}/assets

This operation purges a cached asset or invalidates the cache.

.. note::
   The rate limit for this operation is 20 requests per minute.

When you specify a purge, this operation purges the current version of the asset that has been cached at the edge node. Currently in Rackspace CDN, purges require the URL of the file to purge. Use of wildcards is not in effect.

.. note::
   By default, the ``hard`` query parameter is set to ``true`` to purge the assets. You can set ``hard`` to ``false`` to invalidate the cache, which might be faster than purging.

Following are some characteristics and considerations related to a purge of a cached asset:

-  You specify a purge by setting ``hard = true`` in the URL as a query parameter.

-  If you do not specify a setting for ``hard``, ``hard = true`` is the default.

-  A purge takes more time than a cache invalidation (roughly 10 minutes per file after the API request returns a 202 (Success) response code.

-  A purge deletes content from the CDN edge nodes’ hard drives.

-  A purge should be used with sensitive material only.

-  A purge enable the removal of a single file at a time.

Following are some characteristics and considerations related to a cache invalidation:

-  You specify a cache invalidation by setting ``hard = false`` in the URL as a query parameter.

-  A cache invalidation takes less time than a purge (roughly 60 seconds per request after the API request returns a 202 (Success) response code.

-  A cache invalidation forces content expiration on the CDN edge nodes’ hard drives.

-  A cache invalidation allows the basic wildcard ‘*’ and subdirectories so you can execute against multiple files at one time.

-  When re-querying the origin for content, a cache invalidation sends the ``If-Modified-Sinced`` header with the request.

-  How the CDN responds is based on the origin’s response as follows:

   -  When the origin responds with a 200, the Akamai server updates its cache and time stamp, and the new object is served to the client.

   -  When the origin responds with a 304, the Akamai server updates its time stamp, and the stale object is served to the client.

   -  When the origin responds with a 404, the Akamai server updates its cache (with the error response) and time stamp (404’s are cached for 10s by default), and the 404 is served to the client.

   -  When the origin responds with 5xx, the Akamai server updates its time stamp, and the stale object is served to the client.

   -  When the origin cannot be reached, the Akamai server updates its time stamp, and the stale object is served to the client.

.. note::
   Akamai's SLA for cache invalidation, which is the time when the policy change takes effect until the actual invalidation, is approximately 60 seconds. For Rackspace CDN, additional time must be added to that SLA to account for the time taken to deploy the service.

The following table shows the possible response codes for this operation.


+--------------------------+-------------------------+------------------------+
|Response Code             |Name                     |Description             |
+==========================+=========================+========================+
|202                       |Accepted                 |Success.                |
+--------------------------+-------------------------+------------------------+

Request
-------

The following table shows the URI parameters for the request.

+-------------+-------------+-------------------------------------------------+
|Name         |Type         |Description                                      |
+=============+=============+=================================================+
|{project_id} |String       |The project ID for the user. If you do not set   |
|             |             |the ``X- Project-Id header`` in the request, use |
|             |             |``project_id`` in thURI. For example: ``GET      |
|             |             |https://global.cdn.api.rackspacecloud.com/v1.0/  |
|             |             |{project_id}``                                   |
+-------------+-------------+-------------------------------------------------+
|{service_id} |String       |Specifies the service ID that represents         |
|             |             |distributed content. The value is a UUID, such as|
|             |             |96737ae3-cfc1-4c72-be88-5d0e7cc9a3f0, that is    |
|             |             |generated by the server.                         |
+-------------+-------------+-------------------------------------------------+

The following table shows the query parameters for the request.

+--------------------------+-------------------------+------------------------+
|Name                      |Type                     |Description             |
+==========================+=========================+========================+
|url                       |String                   |Specifies the relative  |
|                          |                         |``URL`` of the asset to |
|                          |                         |be purged.              |
+--------------------------+-------------------------+------------------------+
|hard                      |Boolean                  |Specifies a purge when  |
|                          |                         |set to ``true`` or a    |
|                          |                         |cache invalidation when |
|                          |                         |set to ``false``.       |
|                          |                         |Default: ``true``       |
+--------------------------+-------------------------+------------------------+

This operation does not accept a request body.

**Example: Purge a cached asset HTTP request**

.. code::

   DELETE /v1.0/110011/services/96737ae3-cfc1-4c72-be88-5d0e7cc9a3f0/assets?url=/images/logo.png HTTP/1.1
   Host: global.cdn.api.rackspacecloud.com
   X-Auth-Token: 0f6e9f63600142f0a970911583522217

Response
--------

This operation does not return a response body.

**Example: Purge a cached asset HTTP response**

.. code::

   HTTP/1.1 202 Accepted
   Location: https://global.cdn.api.rackspacecloud.com/v1.0/services/96737ae3-cfc1-4c72-be88-5d0e7cc9a3f0
