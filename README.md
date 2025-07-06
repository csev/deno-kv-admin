# deno-kv-admin

This is a set of web service endpoints to support `kvadmin.py` in the Deno modult
of PostgreSQL for Everybody (www.pg4e.com)

# Installation Instructions

You will need a github account to use Deno Deploy. To create or access your
Deno Deploy dashboard, go to:

https://dash.deno.com/

At the `Overview` section, create a `New Playground`.  Copy the contents of this Deno
application:

https://github.com/csev/deno-kv-admin/blob/main/main.ts

And paste it into the code panel of your new Deno playground.  Before you `Save and Deploy`,
scroll to the bottom and delete the `Deno.cron()` method so your data does not get wiped out
every hour.  Or you could change the CRON string to sowthing like ""0 0 1 * *" to reset your
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

Download the following files to a fiolder on your computer.  If you you have been taking the PostgreSql for
Everybody course you may already have a folder and a `hidden.py` with your secrets.

https://www.pg4e.com/code/kvadmin.py

https://www.pg4e.com/code/hidden-dist.py
