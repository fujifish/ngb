# ngb - Nab, Grab n' Benchmark a URL

__ngb__ is a commandline utility for testing and benchmarking a list of URLs.
It is similar to [Apache Bench](http://httpd.apache.org/docs/trunk/programs/ab.html), but
can also perform additional checks on the content returned from the server.

__ngb__ runs iterations of tests against a list of URLs, where each URL is a separate test.
Every iteration runs all the test URLs before continuing to the next iteration.

Once all requests are executed for a test, the next test is executed (the next URL).
Once all tests are finished, the next iteration is executed.

You can specify the total number of requests per test, with the number of concurrent requests to use.

__ngb__ by default checks that the content returned from the server is the same in all responses, and will report an error
for responses that do not match the baseline response. The baseline response is the first response returned from the
server.

__ngb__ reports at the end of every test the average request duration, and details all the errors if there were any.
 
## Installation

```bash
npm install -g ngb
```

## Usage

```
> ngb
Usage: ngb [OPTIONS] URL [URL...]

  URL          A URL to perform a test against

Options:
  -m <method>                  the http method to use for the request. default is GET.
  -i <number>                  the number of iteration to run all the tests. default is 1.
  -d <number>                  delay in seconds between test iterations. default is 30.
  -c <number>                  concurrent requests per test per iteration. default is 1.
  -n <number>                  total number of requests per test per iteration. default is 1.
  -t <number>                  request timeout in milliseconds to consider as an error. default is 10000
  -p <url>                     proxy as http[s]://[username:password]@hostname_or_ip[:port]. default is no proxy
  --content <file or string>   the request content to send to the server. may be a string to send or a js file that exports a function that will be used to return the content
  --content-type <type>        the content type that is sent to the server. default is application/json
  --check-content <true|false> whether to check the response body and report an error for responses that do not match the expected content. default is true.
  --ntlm <domain>              NTLM domain if the proxy auth is ntlm
  --report-errors <url>        report errors to the specified url using HTTP POST. reported errors are formatted as logstash JSON messages
  --report-tag <url>           report errors tag value
  --progress                   show requests progress

```

## Examples

* run 3 iterations with 1 second delay against a single url `https://httpbin.org/user-agent` with 10 sequential requests, also showing progress

```
ngb -i 3 -d 1 -n 10 --progress https://httpbin.org/user-agent
```

* run 3 iterations against a single url `https://httpbin.org/user-agent` with a total of 100 requests and 10 concurrent

```
> ngb -i 3 -d 1 -n 100 -c 10 --progress https://httpbin.org/user-agent
```

* run through a proxy with authentication against a single url `https://httpbin.org/user-agent`

```
ngb -n 100 -c 10 -p http://admin:pass123@168.192.0.2:8081 https://httpbin.org/user-agent
```

* run against a single url `https://httpbin.org/user-agent` without checking for content response errors

```
ngb -n 100 -c 10 --check-content false https://httpbin.org/user-agent
```

## Sample Output

```
---- [2016-12-09 14:49:09.291] Starting Test on https://httpbin.org/user-agent [iteration 1] ----
request 0 done (721ms)
request 1 done (520ms)
request 2 done (172ms)
request 3 done (173ms)
request 4 done (170ms)
request 5 done (195ms)
request 6 done (170ms)
request 7 done (172ms)
request 8 done (174ms)
request 9 done (173ms)
Test URL: https://httpbin.org/user-agent
Average Duration: 173ms
Total Requests: 10
Total Errors: 0
Request Baseline: {
  "headers": {
    "server": "nginx",
    "date": "Fri, 09 Dec 2016 14:49:09 GMT",
    "content-type": "application/json",
    "content-length": "143",
    "connection": "keep-alive",
    "access-control-allow-origin": "*",
    "access-control-allow-credentials": "true"
  },
  "length": 143,
  "hash": "81a6e251177ab0488dbc7fda4384327f5ac8ccde"
}
Test Options: {
  "concurrent": 1,
  "total": 10,
  "iterations": 3,
  "delay": 1,
  "timeout": 10000,
  "progress": true,
  "checkContent": true,
  "urls": [
    "https://httpbin.org/user-agent"
  ],
  "method": "GET",
  "content": null,
  "headers": {
    "Accept-Encoding": "gzip",
    "Accept": "*/*",
    "User-Agent": "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_10_3) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/45.0.2454.99 Safari/537.36"
  }
}
---- [2016-12-09 14:49:11.940] Ended Test on https://httpbin.org/user-agent [iteration 1] ----
---- Delaying next iteration for 1 seconds ----
---- [2016-12-09 14:49:12.944] Starting Test on https://httpbin.org/user-agent [iteration 2] ----
request 0 done (736ms)
request 1 done (521ms)
request 2 done (179ms)
request 3 done (169ms)
request 4 done (179ms)
request 5 done (170ms)
request 6 done (179ms)
request 7 done (315ms)
request 8 done (182ms)
request 9 done (172ms)
Test URL: https://httpbin.org/user-agent
Average Duration: 172ms
Total Requests: 10
Total Errors: 0
Request Baseline: {
  "headers": {
    "server": "nginx",
    "date": "Fri, 09 Dec 2016 14:49:13 GMT",
    "content-type": "application/json",
    "content-length": "143",
    "connection": "keep-alive",
    "access-control-allow-origin": "*",
    "access-control-allow-credentials": "true"
  },
  "length": 143,
  "hash": "81a6e251177ab0488dbc7fda4384327f5ac8ccde"
}
Test Options: {
  "concurrent": 1,
  "total": 10,
  "iterations": 3,
  "delay": 1,
  "timeout": 10000,
  "progress": true,
  "checkContent": true,
  "urls": [
    "https://httpbin.org/user-agent"
  ],
  "method": "GET",
  "content": null,
  "headers": {
    "Accept-Encoding": "gzip",
    "Accept": "*/*",
    "User-Agent": "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_10_3) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/45.0.2454.99 Safari/537.36"
  }
}
---- [2016-12-09 14:49:15.746] Ended Test on https://httpbin.org/user-agent [iteration 2] ----
---- Delaying next iteration for 1 seconds ----
---- [2016-12-09 14:49:16.751] Starting Test on https://httpbin.org/user-agent [iteration 3] ----
request 0 done (827ms)
request 1 done (515ms)
request 2 done (180ms)
request 3 done (173ms)
request 4 done (183ms)
request 5 done (196ms)
request 6 done (181ms)
request 7 done (247ms)
request 8 done (182ms)
request 9 done (171ms)
Test URL: https://httpbin.org/user-agent
Average Duration: 171ms
Total Requests: 10
Total Errors: 0
Request Baseline: {
  "headers": {
    "server": "nginx",
    "date": "Fri, 09 Dec 2016 14:49:17 GMT",
    "content-type": "application/json",
    "content-length": "143",
    "connection": "keep-alive",
    "access-control-allow-origin": "*",
    "access-control-allow-credentials": "true"
  },
  "length": 143,
  "hash": "81a6e251177ab0488dbc7fda4384327f5ac8ccde"
}
Test Options: {
  "concurrent": 1,
  "total": 10,
  "iterations": 3,
  "delay": 1,
  "timeout": 10000,
  "progress": true,
  "checkContent": true,
  "urls": [
    "https://httpbin.org/user-agent"
  ],
  "method": "GET",
  "content": null,
  "headers": {
    "Accept-Encoding": "gzip",
    "Accept": "*/*",
    "User-Agent": "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_10_3) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/45.0.2454.99 Safari/537.36"
  }
}
---- [2016-12-09 14:49:19.608] Ended Test on https://httpbin.org/user-agent [iteration 3] ----
```