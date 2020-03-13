# servo-ab
Optune servo driver for Apache Benchmark


## Supported environment variables:

* `AB_TEST_URL` - URL to measure
* `AB_HEADERS` - JSON "list" obejct key:value headers: e.g.:
```
["Host: hostname.local", "X-AUTH-TOKEN: aTokenForAuthentication"]
```
* `AB_N_THREADS` - Number of multiple requests to perform at a time.
* `AB_N_REQUESTS` - Number of requests to perform for the benchmarking session.
* `AB_T_LIMIT` - Maximum number of seconds to spend for benchmarking.
* `AB_T_WARMUP` - Number of seconds to wait after start to perform measurement.

Note: Control parameters can also be defined in code.  There are defaults for
AB_N and AB_T parameters if not specified as environment variables.

## Supported control parameters:

* `load` - load configuration object, containing the following attributes:
	* `test_url` - URL to measure (overrides value from environment variable)
	* `headers` - list of arbritrary headers to be sent, example: `["Host: back.example.com", "X-AUTH-TOKEN: ThisShouldBeAnAuthToken"]`
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
Pass Header parameter(s) as an environment variable (*NOTE*: headers are):
```
export AB_TEST_URL=http://localhost:8090/?busy=5
export AB_HEADERS='["Host: back.example.com", "X-AUTH-TOKEN: ThisShouldBeAnAuthToken"]'
measure back <<EOF
{}
EOF
```
Pass multiple Header parameters to the measure command as JSON (_note_: multiple headers need to be in a proper JSON list):
```
export AB_TEST_URL=http://localhost:8090/?busy=5
measure front <<EOF
{"control":"load":{"headers":["Host: back.example.com", "X-AUTH-TOKEN: ThisShouldBeAnAuthToken"]}}}
EOF
```
