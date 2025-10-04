linecgi
=======

a line-oriented cgi-like protocol

kinda like fastcgi but stupider and line oriented instead of binary

a server can spawn one or more handlers, then it accepts http connections,
and forwards them to the handlers

handlers start with stdin/stdout connected to the server via pipes or sockets

handlers can then just read and write, they don't need to loop on accept

handlers terminate on eof

message format
--------------

```
<decimal value>\n             # message id
<decimal value>\n             # number of lines in this messages after this one
```

protocol
--------

each server message contains:
- a list of newline separated headers (possibly empty)
- an empty line
- a body (possibly empty)

clients reply with http

the headers format is configurable and not particularly important

example
-------

```
>0\n                          # server sends id
>4\n                          # this message will contain 4 more lines
>METHOD GET\n                 # headers (configurable)
>URI /myip\n
>REMOTE_ADDR 1.2.3.4\n
>                             # separator (body is empty)

<0\n                          # client replies to message id 0
<3\n                          # this message will contain 3 more lines
<HTTP/1.1 200 OK\r\n          # actual http reply
<\r\n
<1.2.3.4\r\n
```
