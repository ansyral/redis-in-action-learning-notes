This chapter introduces several application cases for Redis.

1. Login cookie

   Cookies are small pieces of data that websites send to web browser to store and resend on every request to the service.

   Token cookies use a series of random bytes as data in the cookie. On the server, the token is used as a key to look up the user who owns that token by querying a database of some kind.

   If the server also want to record information like how long the user has been browsing, or how many items they've looked at, then the database that records the token-user/usage mapping, is a high-write case. If the site has a large load at peak time, then the database has to be sharded to meet the demand.

   If use Redis, we only need

   + `login:` hashtable to record token-user mapping
   + `recent:{token}` sorted set of timestamps that the user who owns the token visited
   + `viewed:{token}` sorted set of items that the user who owns the token viewed

   and we could have a daemon process to clean up some old data when it exceeded some memory limit.

    ```python
    QUIT = False
    LIMIT = 10000000

    def clean_sessions(conn):
        while not QUIT:
        size = conn.zcard('recent:')
        if size <= LIMIT:
            time.sleep(1)
            continue

        end_index = min(size - LIMIT, 100)
        tokens = conn.zrange('recent:', 0, end_index-1)

        session_keys = []
        for token in tokens:
            session_keys.append('viewed:' + token)

        conn.delete(*session_keys)
        conn.hdel('login:', *tokens)
        conn.zrem('recent:', *tokens)
    ```

2. Shoping cart