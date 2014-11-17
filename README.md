# paketannahme

paketannahme stores files uploaded via HTTP on disk.

It's simple enough that it can be used with curl.</br>
It doesn't serve the uploaded files back - it's one-way.</br>
It supports HTTP authentication, size and rate limiting.</br>

Author: Felix Kaiser (felix.kaiser@fxkr.net)</br>
License: MIT (see LICENSE)


## Quickstart

    export GOPATH=$(pwd) && go get && go build
    ./paketannahme &
    echo hello, world | curl localhost:8080 --data-binary @-


## How to build

    export GOPATH=$(pwd)
    go get
    go build


## How to configure

Command line options:

    -addr=""          IP address to bind to (default: any)
    -burst=16         rate limit: maximum burst size
    -config=""        config file path
    -delay=30         rate limit: seconds per request
    -directory=""     path to directory to drop files in
    -password=""      password for HTTP authentication (dangerous!)
    -port=8080        TCP port to bind to
    -size-limit=1024  maximum file size, in kB
    -url=""           base url to tell clients
    -user=""          username for HTTP authentication (dangerous!)

All of these options can also be put in a config file.
To load a config file, use the `-config <path>` parameter.
A config file could look like this:

    port = 1234
    user = demo
    password = demo

Security advice: do not specify user/password on the command line!


## How to use

Assuming you have `paketannahme` running on `localhost:8080` with `-user felix` and `-password secret`:

    echo hello, world | curl http://felix:secret@localhost:8080 --data-binary @-

`paketannahme` will acknowledge receipt:

    Accepted 13 bytes.

    Folder: 2014-11-17_15:50:18_1f7b169c

    Add related files? Pipe to:

    curl -u"felix":"secret" http://localhost:8080/2014-11-17_15:50:18_1f7b169c --data-binary @-

You can use the new URL to upload files to the same folder.

If you forget how to use this, just `GET` the root URL. It'll return a copy/pasteable line:

    % curl http://felix:secret@localhost:8080/
    curl -u"felix":"secret" http://localhost:8080 --data-binary @-
