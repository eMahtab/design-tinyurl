# Design Tinyurl


!["Tinyurl"](tinyurl.PNG?raw=true)


## Design :
1. Each application instance on startup request the token service , to provide a range of tokens (e.g. 100 tokens), that it can use to generate short urls.

2. Also when a app server is out of tokens, then also it asks the token service to provide the range of tokens that it can use.

3. One small issue with the above design is, if a app server crashes in the middle than the range of tokens the server was assigned will be lost.
