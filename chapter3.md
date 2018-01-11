A list of commands commonly used:
+ String
  * get
  * set
  * incr
  * decr
  * incrby
  * decrby
  * incrbyfloat
  * getrange
  * setrange
  * append
  * getbit
  * setbit
  * bitcount
  * bitop
+ List
  * rpush
  * lpush
  * rpop
  * lpop
  * brpop
  * blpop
  * lindex
  * lrange
  * ltrim
  * rpoplpush
  * brpoplpush
+ Set
  * sadd
  * srem
  * sismember
  * sismember
  * scard
  * smembers
  * srandmember
  * spop
  * smove
  * sdiff
  * sdiffstore
  * sinter
  * sinterstore
  * sunion
  * sunionstore
+ Hash
  * hmget
  * hmset
  * hdel
  * hlen
  * hexists
  * hkeys
  * hvals
  * hgetall
  * hincrby
  * hincrbyfloat
+ Sorted set
  * zadd
  * zrem
  * zcard
  * zincrby
  * zcount
  * zrank
  * zscore
  * zrange
  * zrevrank
  * zrevrange
  * zrangebyscore
  * zrevrangebyscore
  * zremrangebyrank
  * zremrangebyscore
  * zinterstore
  * zunionstore
+ Publish/Subscribe(pushing way)

  When describing the limit of P/S of Redis, the author mentions that:

  1. might lose data if client that has subscribed is disconnected for network issue or something else.
  2. clients that read messages so slow that they could make Redis itself keep a large outgoing buffer.

  *I'm a bit confused here. Is Redis messaging a push way or pull way? The first limit seems to mean that it is a push way while the second limit seems to mean that is is a pull way as Redis server needs to maintain the message queue. Let me revisit this later.*

  * subscribe
  * unsubscribe
  * publish
  * psubscribe
  * punsubscribe
+ Sort
  * sort
  > it could manipulate list, set, sorted set.
+ Transaction
  * watch
  * multi
  * exec
  * unwatch
  * discard

  When seeing `multi`, Redis would queue up commands from that same connection until it sees `exec`, and then Redis would execute the queued commands sequently without interruption.

  redis-py client use `pipeline` to wrap this:

  ```python
    def trans():
        pipeline = conn.pipeline()
        pipeline.incr('trans:')
        time.sleep(.1)
        pipeline.incr('trans:', -1)
        print pipeline.execute()[0]
   
   if 1:
        for i in xrange(3):
            threading.Thread(target=trans).start()
        time.sleep(.5)
  ```
+ Expiring keys
  * persist
  * ttl
  * expire
  * expireat
  * pttl
  * pexpire
  * pexpireat

  These commands only support expiring the entire value of a key. If you'd like to expire part of data, you can use zset and expire according to timestamp.
