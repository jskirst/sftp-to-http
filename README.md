# Sftp-to-Http

[![Code Climate](https://codeclimate.com/github/starfighterheavy/sfto-to-http/badges/gpa.svg)](https://codeclimate.com/github/starfighterheavy/sfto-to-http)

SSH2 server that posts files to http endpoint, based on the ssh2 (mscdex/ssh2) module written by Brian White <mscdex@mscdex.net>

[Based on mscdex/ssh2](https://github.com/mscdex/ssh2)

# Getting Started

First, set your HTTP_SERVER_URL environment variable to the URL you would like the file contents posted to, the SFTP_SERVER_PORT you want your SFTP server to listen to, and the path to your public key file path.

```
export HTTP_SERVER_URL=http://example.com
export SFTP_SERVER_PORT=2222
export SFTP_SERVER_PORT_KEY_PATH=/home/.../keypath
```

Clone the repo to your local workspace.

```
git clone git@github.com:jskirst/sftp-to-http.git
```

Inside your project directory, run npm install.

```
npm install
```

Create an SSH key pair. You will use the public key to `sftp` into the server. The private key should be named `host.key` and placed in the root of the project directory.

```
ssh-keygen
```

Start your server:

```
npm run-script run
```

This will start a running server listening on port 2222. To connect to the server (assuming you've added the host to your IdentifyFile):

```
sftp -p 2222 localhost
```

To run a basic test, you can put the file `examples/test.txt`. It doesn't matter what the destination is, since the file will not be written to disk.

```
put /[pathtoproject]/examples/test.txt /anywhere
```

You should see (something like) the following return:

```
Uploading ./test.txt to /tmp/foo.txt
./test.txt                                                                100%  737     0.7KB/s   00:00
```

And you should see a successful multipart POST call made to the HTTP_SERVER_URL with `Content-Type: text/plain`.

Note that the POST call will happen asynchronously to the SFTP request you make and therefore may arrive after the SFTP call completes.
