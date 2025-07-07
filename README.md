# deno-kv-admin

This is a set of web service endpoints to support  [kvadmin.py](https://www.pg4e.com/code/kvadmin.py) in
the [Deno](https://docs.deno.com/deploy/kv/manual/) module
of PostgreSQL for Everybody (www.pg4e.com)

It is designed to be installed in a developer account on [Deno Deploy](https://dash.deno.com).

# Installation Instructions

You will need a github account to use Deno Deploy. To create or access your
Deno Deploy dashboard, go to:

https://dash.deno.com/

At the `Overview` section, create a `New Playground`.  Copy the contents of this file into the new
Deno Playgrund application:

https://github.com/csev/deno-kv-admin/blob/main/main.ts

Paste it into the code panel of your  Deno playground.  Before you `Save and Deploy`,
scroll to the bottom and delete the `Deno.cron()` method so your data does not get wiped out
every hour.  Or you could change the CRON string to something like ""0 0 1 * *" to clear your
data once per month.

Also note the `checkToken()` code near the end.  The autograder will ask you to change the token
to a particular value in order to submit your assignments for evaluation and credit.

Once you have removed the CRON entry, you can `Save and Deploy`. It can take 30 seconds to get your code
deployed to the Deno Deploy cloud.  Check the logs to make sure you did not introduce a syntax error.

Once it is up you will get a URL to access your appilication like:

https://comfortable-starling-12.deno.dev

If you access this URL, you will (correctly) get a `404 Not Found`.  

# Initial testing

You can test the application by adding `/dump` to the end of the URL as follows (make sure to change the host
name to match your Deno instance):

https://comfortable-starling-12.deno.dev/dump

You should get an output that looks as follows:

    {
      "method": "GET",
      "url": "https://comfortable-starling-12.deno.dev/dump",
      "path": "/dump",
      "headers": {
        "accept": "text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8",
        "accept-encoding": "gzip, deflate, br, zstd",
        "accept-language": "en-US,en;q=0.5",
        "host": "comfortable-starling-12.deno.dev",
        "priority": "u=0, i",
        "sec-fetch-dest": "document",
        "sec-fetch-mode": "navigate",
        "sec-fetch-site": "none",
        "sec-fetch-user": "?1",
        "te": "trailers",
        "traceparent": "00-ac9e975daac67625527f3a3da6b1dfdd-5274315352c7753c-01",
        "upgrade-insecure-requests": "1",
        "user-agent": "Mozilla/5.0 (Macintosh; Intel Mac OS X 10.15; rv:140.0) Gecko/20100101 Firefox/140.0"
      },
      "query": {},
      "body": ""
  }

The `/dump` url does not require a token, but the URLs that access Deno KV require a token.
You can test to see if your token works by going to the following URL (make sure to change the host
name and token to match your Deno instance):

https://comfortable-starling-12.deno.dev/kv/list/books?token=123

If it works and you have no books the output will look as follows:

    {
      "records": [],
      "cursor": ""
    }

Which means "no orecords exist with prefix of books".  If your token is wrong you should get a
`401` response code and a message of

    Missing or invalid token

At this point you have verified that your KVAdmin web service end points are up and running.

# Using kvadmin.py

We have built a simple Python client for this server that allows you to execute simple DENO KV commands
like `set`, `get`, `list`, or `delete` from the command line on your computer.  This is a very simplistic
equivalent of the `psql` command line client for PostgreSQL.

Download the following files to a folder on your computer.  If you you have been taking the PostgreSql for
Everybody course you may already have a folder and a `hidden.py` with your secrets.

https://www.pg4e.com/code/kvadmin.py

https://www.pg4e.com/code/hidden-dist.py

If you don't already have a `hidden.py`, copy `hidden-dist.py` to `hidden.py` and edit the `deno()` function
to set your host name and token value:

    def denokv():
        return { 'token' : '99123',
             "url": "https://comfortable-starling-12.deno.dev" }

Then navigate into the folder using terminal, command line, or a shell and run `python kvadmin.py` or 
`python3 kvadmin.py`.  You might see the following:

    python kvadmin.py 
    Verifying connection to https://comfortable-starling-12.deno.dev/dump
    
    Unable to communicate with Deno.  Sometimes it takes a while to start the
    the Deno instance after it has been idle.  You might want to access the url
    below in a browser, wait 30 seconds, and then restart kvadmin.
    
    https://comfortable-starling-12.deno.dev/dump

This checks if your `denokv()` values are correct and being read.  Sometimes if your Deno application 
has been idle for a while it can take up to 30 seconds to cold start your application.  If you pay
for your Deno deployment or if the application is not idle - startup is very quick.

After about 30 seconds, you should start `kvadmin.py` and get the `Enter Command:` prompt. The following
are the `kvadmin.py` commands and a few sample commands:

    $ python kvadmin.py 
    Verifying connection to https://kv-admin-api.pg4e.com/dump

    Enter Command: help
      quit
      samples
      set /books/Hamlet
      get /books/Hamlet
      list /books
      delete /books/Hamlet
      delete_prefix /books

    Enter command: samples
    
    {"author": "Bill", "title": "Hamlet", "isbn": "42", "lang": "ang"}
    
    Enter command: set /books/Hamlet 
    Enter json (finish with a blank line:
    {"author": "Bill", "title": "Hamlet", "isbn": "42", "lang": "ang"}
    
    https://kv-admin-api.pg4e.com/kv/set/books/Hamlet?token=42
    {
      "ok": true,
      "versionstamp": "010000000591ef100000"
    }
    
    Enter command: list /books
    https://kv-admin-api.pg4e.com/kv/list/books?token=42
    200
    {
        "records": [
            {
                "key": [
                    "books",
                    "Hamlet"
                ],
                "value": {
                    "author": "Bill",
                    "title": "Hamlet",
                    "isbn": "42",
                    "lang": "ang"
                },
                "versionstamp": "010000000591ef100000"
            }
        ],
        "cursor": ""
    }
    
    Enter command: quit

If the auto grader tells you to change your `token` value, make sure to change it on your
Deploy Playground instance and do a `Save and Deploy` and then also change your `hidden.py` so
that your `kvadmin.py` continues to work.
