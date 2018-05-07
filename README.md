# servo-ab
Optune servo driver for Apache Benchmark

Supported control parameters:

* `load` - load configuration object, containing the following attributes:
	* `test_url` - URL to measure (**REQUIRED**)
	* `n_threads` - number of concurrent requests, default 10
	* `n_requests` - total number of requests to send, default 2000
	* `user` - user name to authenticate with, default none
	* `password` - user password to authenticate with, default none

Note: this driver requires `measure.py` base class from the Optune servo core. It can be copied or symlinked here as part of packaging