# Design Tinyurl


!["Tinyurl"](tinyurl.PNG?raw=true)


## Design :
1. Each application instance on startup request the token service , to provide a range of tokens (e.g. 100 tokens), that it can use to generate short urls.

2. Also when a app server is out of tokens, then also it asks the token service to provide the range of tokens that it can use.

3. One small issue with the above design is, if a app server crashes in the middle than the range of tokens the server was assigned will be lost.


## Token Service :

!["counter table"](counter.PNG?raw=true)

Counter table stores the value of count. Whenever an app instance requests the token service to generate the range of tokens, token service starts a transaction
and increment the value of count in the counter table by some configured number (e.g. say 100).


For example if app server 1 requests the token service to provide the range of tokens that it can use, and say the value of count in the counter table is currently 0.
And the number of tokens to be allocated is configured as say 100 in one single request to Token service. It means app server 1 can now use token starting from 0 to 100 (0+100), and the count value in the counter table will now be updated to 101.

Now if we get a new request for token generation, we will allow it to use the values in the range 101 to 201 (101+100). And the value of count will be updated to 202

We read the count value from counter table and do the update in a transaction, and commit the transaction. As multiple servers might make the request to token service at a single point of time.

# Short URL Generation
```
urls
-------------------------
short_url |  varchar(30) | Primary Key
long_url | varchar(500) |
```
Application generates the base62/base64 value from the current count. And increment the value of count.

e.g. stackoverflow.com gets shortened to tiny.com/hyd28re

## References :

