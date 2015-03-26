#URL - Uniform Resource Locator

##URL Components
example: "http://www.example.com/home"

- `http`: Tell the web client to use the Hypertext Transfer Protocol or HTTP to make a request.
- `www.example.com`: URL
- `/home/`: **URL Path**. It shows that local recource is being requested.

**Unless different port number is specified, port 80 will be used by default in normal HTTP requests**

##Query Strings/Parameters
A Query string or parameter is part of the URL and usually contains some form of data to be sent to the server.
`	http://www.example.com?search=ruby&results=10`

- `?`  This is a reseved character that marks the start of the query string
- `search=ruby`This is a parameter name/value pair
- `&`This is a reserved character, used when adding mire parameters to query string.
- `result=10` also parameter name/value pair

**Query strings are passed in through the URL, they are only used in HTTP GET requestd**

##Bad for Query String
- Query Strings have a maximum length.
- Query Strings are visible on URL. So you won't transfer usename or password on it's
- Space and special characters like `&` cannot be used with query strings. They must be URL encoded, which we'll talk about next.


##URL Encoding
URL encoding serves the purpose of replacing these non-conforming characters with `%` symbol followed by two hexadecimal digits that represent the ASCII code of the character.

ex: http://www.thedesignshop.com/shops/tommy%20hilfiger.html

`%20`is the `Space` encoding



