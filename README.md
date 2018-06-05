# servo-ab
Optune servo driver for Apache Benchmark


Supported environment variables:

* `AB_TEST_URL` - URL to measure

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