1. Install Redis in single node and install Redis python client to play

    * Install Redis and run in single node
        ```
        ~: $ wget -q http://download.redis.io/releases/redis-4.0.6.tar.gz
        ~: $ tar -xzf redis-4.0.6.tar.gz
        ~: $ cd redis-4.0.6
        ~/redis-4.0.6: $ make
        ~/redis-4.0.6: $ sudo make install
        ~/redis-4.0.6: $ redis-server redis.conf
        ```

        it would show like something like below: ![start redis server](./images/install-start-server.PNG)

    * Install Redis python client

        Open a new terminal, this step is to install redis client.
        For this part, I didn't follow the steps in the book as it is kind of old. I just follow the readme in [redis-py](https://github.com/andymccurdy/redis-py).

        I also checked it worked: ![test redis](./images/install-test-redis.PNG)
