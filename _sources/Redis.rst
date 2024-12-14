=====
Redis
=====

* no-sql database, doesn't have tables or documents
* data is stored in key, value pairs
* runs inside memory and much more volatile
* used mainly for caching, on top of other database
* stored as string by default

.. code-block:: sh

   SET key value
   GET key   # GET only works for string
   DEL key
   EXISTS key  # return 1 or 0
   KEYS *  # get all keys
   FLUSHALL  # clear all
   TTL key  # time to live, -1 for forever, -2 for gone
   EXPIRE key  # set time for expire
   SETEX key time value  # set key with expire time in seconds


* also support for handling arrays in list form and hashes for json
* every key in a set has to be unique

.. code-block:: sh

   LPUSH key value  # add item at the start/left of the array
   LRANGE key start end  # 0 -1 to print all
   RPUSH key value  # add item at the end/right of the array
   LPOP key  # remove item at the start/left
   RPOP key  # remove item at the end/right
   
   SADD key value  # add item to a set
   SMEMBERS key  # list all items in a set
   SREM key value  # remove a key from a set
   
   HSET key field value  # add a value to a field of a key
   HGET key field  # get a value of a field from a key
   HGETALL key  # get all fields and values from a hash, prints field first
   HDEL key field  # delete a field from a key
   HEXISTS key field  # return 1 or 0

