# servo-ab
Optune servo driver for Apache Benchmark


Supported environment variables:

* `AB_TEST_URL` - URL to measure
* `AB_EXTRA_HEADERS` - comman separated key:value headers: e.g.:
```host:hostname.local,X-AUTH-TOKEN:aTokenForAuthentication```

Supported control parameters:

* `load` - load configuration object, containing the following attributes:
	* `test_url` - URL to measure (overrides value from environment variable)
	* `n_threads` - number of concurrent requests, default 10
	* `n_requests` - limit on number of requests to send, default 100,000,000
	* `t_limit` - time limit in seconds for measurement, default 180
	* `t_warmup` - time in seconds for warmup, default 30
	* `user` - user name to authenticate with, default none
	* `password` - user password to authenticate with, default none

Note: a test URL must be defined either in the environment variable or in the `load` parameters

Note:  by default the measurement will run for the specified `t_limit` up to `n_requests` requests, terminating on whatever limit is first encountered.  If `t_limit` is `0`, then the measurement will run for `n_requests` requests.

Note: this driver requires `measure.py` base class from the Optune servo core. It can be copied or symlinked here as part of packaging

## Testing the driver

Launch a simple test app that does some processing (e.g. docker run --rm -d -p 8080:8080 opsani/co-http)

Run a test using the defaults from the command line:

```
export AB_TEST_URL=http://localhost:8080/?busy=5
measure test <<EOF
{}
EOF
```

If your test app is loaded in an enviornment with multiple routes (e.g. Rancher loadbalancer, or Kubernetes with Ingress), you can pass extra header to point to specific services.  As an example
if you load the test app in Rancher from the https://github.com/opsani/servo-rancher README):

```
export AB_TEST_URL=http://localhost:8090/?busy=5
measure front <<EOF
{}
EOF
```
Pass multiple Header parameters as an environment variable (_note_: multiple headers are comma separated):
```
export AB_TEST_URL=http://localhost:8090/?busy=5
export AB_EXTRA_HEADERS=host:back.example.com,x-auth-token:ThisShouldBeAnAuthToken
measure back <<EOF
{}
EOF
```
Pass multiple Header parameters to the measure command as JSON (_note_: multiple headers need to be in a proper JSON list):
```
export AB_TEST_URL=http://localhost:8090/?busy=5
measure front <<EOF
{"control":"load":{"extra_headers":["host:back.example.com","x-auth-token:ThisShouldBeAnAuthToken"]}}}
EOF
```
