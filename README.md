http-console
============

> Speak HTTP like a local

Talking to an HTTP server with `curl` can be fun, but most of the time it's a `PITA`.

`http-console` is a simple and intuitive interface for speaking the HTTP protocol.

*PS: HTTP has never been this much fun.*

synopsis
--------

![http-console](http://dl.dropbox.com/u/251849/http-console.png)

installation
------------

*http-console* was written for [node](http://nodejs.org), so make sure you have that installed
first. Then you need [npm](http://github.com/isaacs/npm), node's package manager.

Once you're all set, run:

    $ npm install http-console

It'll download the dependencies, and install the command-line tool in `/usr/local/bin`.

introduction
------------

Let's assume we have a [CouchDB](http://couchdb.apache.org) instance running locally.

### connecting #

To connect, we run `http-console`, passing it the server host and port as such:

    $ http-console 127.0.0.1:5984 

To have line-editing and command history, we can run it through `rlwrap`:

    $ rlwrap http-console 127.0.0.1:5984

### navigating #

Once connected, we should see the *http prompt*:

    http://127.0.0.1:5984/>

server navigation is similar to directory navigation, except a little simpler:

    http://127.0.0.1:5984/> /logs
    http://127.0.0.1:5984/logs> /46
    http://127.0.0.1:5984/logs/46> ..
    http://127.0.0.1:5984/logs> ..
    http://127.0.0.1:5984/>

### requesting #

HTTP requests are issued with the HTTP verbs *GET*, *PUT*, *POST*, *HEAD* and *DELETE*, and
a relative path:

    http://127.0.0.1:5984/> GET /
    HTTP/1.1 200 OK
    Date: Mon, 31 May 2010 04:43:39 GMT
    Content-Length: 41

    {
        couchdb: "Welcome",
        version: "0.11.0"
    }

    http://127.0.0.1:5984/> GET /bob
    HTTP/1.1 404 Not Found
    Date: Mon, 31 May 2010 04:45:32 GMT
    Content-Length: 44

    {
        error: "not_found",
        reason: "no_db_file"
    }

When issuing *POST* and *PUT* commands, we have the opportunity to send data too:

    http://127.0.0.1:5984/> /rabbits
    http://127.0.0.1:5984/rabbits> POST
    ... {"name":"Roger"}

    HTTP/1.1 201 Created
    Location: http://127.0.0.1/rabbits/2fd9db055885e6982462a10e54003127
    Date: Mon, 31 May 2010 05:09:15 GMT
    Content-Length: 95

    {
        ok: true,
        id: "2fd9db055885e6982462a10e54003127",
        rev: "1-0c3db91854f26486d1c3922f1a651d86"
    }

### setting headers #

Sometimes, it's useful to set HTTP headers:

    http://127.0.0.1:5984/> Accept: application/json
    http://127.0.0.1:5984/> X-Lodge: black

These headers are sent with all requests in this session. To see all active headers,
run the `\headers` command:

    http://127.0.0.1:5984/> \headers
    Accept: application/json
    X-Lodge: black

Removing headers is just as easy:

    http://127.0.0.1:5984/> Accept:
    http://127.0.0.1:5984/> \headers
    X-Lodge: black

### cookies #

You can enable cookie tracking with the `--cookies` option flag.
To see what cookies are stored, use the `\cookies` command.

### SSL #

To enable SSL, pass the `--ssl` flag.

### changing hosts #

To change hosts, use the `host` command:

    http://127.0.0.1:80/> host example.org
    http://example.org:80/>

You can also use `http://` and `https://` prefixes:

    http://example.org:80/> host http://example.com
    http://example.com:80/> host https://example.org
    https://example.org:443/>

### quitting #

    Ctrl-D

nuff' said.



