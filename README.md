# Async Python Web Frameworks comparison

https://klen.github.io/py-frameworks-bench/
----------
#### Updated: 2021-10-21

[![benchmarks](https://github.com/klen/py-frameworks-bench/actions/workflows/benchmarks.yml/badge.svg)](https://github.com/klen/py-frameworks-bench/actions/workflows/benchmarks.yml)
[![tests](https://github.com/klen/py-frameworks-bench/actions/workflows/tests.yml/badge.svg)](https://github.com/klen/py-frameworks-bench/actions/workflows/tests.yml)

----------

This is a simple benchmark for python async frameworks. Almost all of the
frameworks are ASGI-compatible (aiohttp and tornado are exceptions on the
moment).

The objective of the benchmark is not testing deployment (like uvicorn vs
hypercorn and etc) or database (ORM, drivers) but instead test the frameworks
itself. The benchmark checks request parsing (body, headers, formdata,
queries), routing, responses.

## Table of contents

* [The Methodic](#the-methodic)
* [The Results](#the-results-2021-10-21)
    * [Accept a request and return HTML response with a custom dynamic header](#html)
    * [Parse path params, query string, JSON body and return a json response](#api)
    * [Parse uploaded file, store it on disk and return a text response](#upload)
    * [Composite stats ](#composite)



<img src='https://quickchart.io/chart?width=800&height=400&c=%7Btype%3A%22bar%22%2Cdata%3A%7Blabels%3A%5B%22blacksheep%22%2C%22muffin%22%2C%22falcon%22%2C%22starlette%22%2C%22emmett%22%2C%22sanic%22%2C%22fastapi%22%2C%22aiohttp%22%2C%22tornado%22%2C%22quart%22%2C%22django%22%5D%2Cdatasets%3A%5B%7Blabel%3A%22num%20of%20req%22%2Cdata%3A%5B515205%2C461415%2C424905%2C349920%2C317925%2C297930%2C269130%2C213735%2C123915%2C106980%2C63660%5D%7D%5D%7D%7D' />

## The Methodic

The benchmark runs as a [Github Action](https://github.com/features/actions).
According to the [github
documentation](https://docs.github.com/en/actions/using-github-hosted-runners/about-github-hosted-runners)
the hardware specification for the runs is:

* 2-core vCPU (Intel® Xeon® Platinum 8272CL (Cascade Lake), Intel® Xeon® 8171M 2.1GHz (Skylake))
* 7 GB of RAM memory
* 14 GB of SSD disk space
* OS Ubuntu 20.04

[ASGI](https://asgi.readthedocs.io/en/latest/) apps are running from docker using the gunicorn/uvicorn command:

    gunicorn -k uvicorn.workers.UvicornWorker -b 0.0.0.0:8080 app:app

Applications' source code can be found
[here](https://github.com/klen/py-frameworks-bench/tree/develop/frameworks).

Results received with WRK utility using the params:

    wrk -d15s -t4 -c64 [URL]

The benchmark has a three kind of tests:

1. "Simple" test: accept a request and return HTML response with custom dynamic
   header. The test simulates just a single HTML response.

2. "API" test: Check headers, parse path params, query string, JSON body and return a json
   response. The test simulates an JSON REST API.

3. "Upload" test: accept an uploaded file and store it on disk. The test
   simulates multipart formdata processing and work with files.


## The Results (2021-10-21)

<h3 id="html"> Accept a request and return HTML response with a custom dynamic header</h3>
<details open>
<summary> The test simulates just a single HTML response. </summary>

Sorted by max req/s

| Framework | Requests/sec | Latency 50% (ms) | Latency 75% (ms) | Latency Avg (ms) |
| --------- | -----------: | ---------------: | ---------------: | ---------------: |
| [blacksheep](https://pypi.org/project/blacksheep/) `1.0.9` | 18337 | 2.84 | 4.48 | 3.49
| [muffin](https://pypi.org/project/muffin/) `0.86.0` | 16114 | 3.64 | 5.06 | 3.95
| [falcon](https://pypi.org/project/falcon/) `3.0.1` | 14977 | 3.41 | 5.61 | 4.25
| [starlette](https://pypi.org/project/starlette/) `0.16.0` | 13115 | 4.13 | 6.24 | 4.86
| [emmett](https://pypi.org/project/emmett/) `2.3.1` | 12873 | 4.07 | 6.41 | 4.95
| [fastapi](https://pypi.org/project/fastapi/) `0.70.0` | 9590 | 5.17 | 8.93 | 6.65
| [sanic](https://pypi.org/project/sanic/) `21.9.1` | 8637 | 5.89 | 9.67 | 7.46
| [aiohttp](https://pypi.org/project/aiohttp/) `3.7.4.post0` | 7426 | 8.41 | 8.56 | 8.62
| [tornado](https://pypi.org/project/tornado/) `6.1` | 3325 | 18.93 | 19.09 | 19.25
| [quart](https://pypi.org/project/quart/) `0.15.1` | 3281 | 20.02 | 20.83 | 19.54
| [django](https://pypi.org/project/django/) `3.2.8` | 1794 | 34.70 | 38.70 | 36.05


</details>

<h3 id="api"> Parse path params, query string, JSON body and return a json response</h3>
<details open>
<summary> The test simulates a simple JSON REST API endpoint.  </summary>

Sorted by max req/s

| Framework | Requests/sec | Latency 50% (ms) | Latency 75% (ms) | Latency Avg (ms) |
| --------- | -----------: | ---------------: | ---------------: | ---------------: |
| [blacksheep](https://pypi.org/project/blacksheep/) `1.0.9` | 10467 | 4.92 | 8.15 | 6.09
| [muffin](https://pypi.org/project/muffin/) `0.86.0` | 10278 | 4.83 | 8.36 | 6.20
| [falcon](https://pypi.org/project/falcon/) `3.0.1` | 10026 | 4.96 | 8.66 | 6.37
| [starlette](https://pypi.org/project/starlette/) `0.16.0` | 7818 | 6.36 | 11.15 | 8.16
| [sanic](https://pypi.org/project/sanic/) `21.9.1` | 7323 | 6.68 | 11.69 | 8.76
| [emmett](https://pypi.org/project/emmett/) `2.3.1` | 6898 | 7.12 | 12.33 | 9.48
| [fastapi](https://pypi.org/project/fastapi/) `0.70.0` | 6209 | 7.99 | 14.48 | 10.29
| [aiohttp](https://pypi.org/project/aiohttp/) `3.7.4.post0` | 4605 | 13.72 | 13.91 | 13.90
| [tornado](https://pypi.org/project/tornado/) `6.1` | 2841 | 22.30 | 22.49 | 22.53
| [quart](https://pypi.org/project/quart/) `0.15.1` | 2123 | 29.62 | 30.46 | 30.14
| [django](https://pypi.org/project/django/) `3.2.8` | 1500 | 42.90 | 46.00 | 42.66

</details>

<h3 id="upload"> Parse uploaded file, store it on disk and return a text response</h3>
<details open>
<summary> The test simulates multipart formdata processing and work with files.  </summary>

Sorted by max req/s

| Framework | Requests/sec | Latency 50% (ms) | Latency 75% (ms) | Latency Avg (ms) |
| --------- | -----------: | ---------------: | ---------------: | ---------------: |
| [blacksheep](https://pypi.org/project/blacksheep/) `1.0.9` | 5543 | 8.96 | 15.65 | 11.54
| [muffin](https://pypi.org/project/muffin/) `0.86.0` | 4369 | 11.37 | 20.25 | 14.65
| [sanic](https://pypi.org/project/sanic/) `21.9.1` | 3902 | 13.17 | 22.22 | 16.40
| [falcon](https://pypi.org/project/falcon/) `3.0.1` | 3324 | 16.84 | 25.60 | 19.42
| [starlette](https://pypi.org/project/starlette/) `0.16.0` | 2395 | 20.75 | 36.67 | 26.73
| [aiohttp](https://pypi.org/project/aiohttp/) `3.7.4.post0` | 2218 | 28.33 | 28.96 | 28.85
| [fastapi](https://pypi.org/project/fastapi/) `0.70.0` | 2143 | 24.18 | 40.50 | 29.86
| [tornado](https://pypi.org/project/tornado/) `6.1` | 2095 | 30.21 | 30.77 | 30.65
| [quart](https://pypi.org/project/quart/) `0.15.1` | 1728 | 36.41 | 37.95 | 37.01
| [emmett](https://pypi.org/project/emmett/) `2.3.1` | 1424 | 41.69 | 48.22 | 45.02
| [django](https://pypi.org/project/django/) `3.2.8` | 950 | 65.57 | 72.84 | 67.21


</details>

<h3 id="composite"> Composite stats </h3>
<details open>
<summary> Combined benchmarks results</summary>

Sorted by completed requests

| Framework | Requests completed | Avg Latency 50% (ms) | Avg Latency 75% (ms) | Avg Latency (ms) |
| --------- | -----------------: | -------------------: | -------------------: | ---------------: |
| [blacksheep](https://pypi.org/project/blacksheep/) `1.0.9` | 515205 | 5.57 | 9.43 | 7.04
| [muffin](https://pypi.org/project/muffin/) `0.86.0` | 461415 | 6.61 | 11.22 | 8.27
| [falcon](https://pypi.org/project/falcon/) `3.0.1` | 424905 | 8.4 | 13.29 | 10.01
| [starlette](https://pypi.org/project/starlette/) `0.16.0` | 349920 | 10.41 | 18.02 | 13.25
| [emmett](https://pypi.org/project/emmett/) `2.3.1` | 317925 | 17.63 | 22.32 | 19.82
| [sanic](https://pypi.org/project/sanic/) `21.9.1` | 297930 | 8.58 | 14.53 | 10.87
| [fastapi](https://pypi.org/project/fastapi/) `0.70.0` | 269130 | 12.45 | 21.3 | 15.6
| [aiohttp](https://pypi.org/project/aiohttp/) `3.7.4.post0` | 213735 | 16.82 | 17.14 | 17.12
| [tornado](https://pypi.org/project/tornado/) `6.1` | 123915 | 23.81 | 24.12 | 24.14
| [quart](https://pypi.org/project/quart/) `0.15.1` | 106980 | 28.68 | 29.75 | 28.9
| [django](https://pypi.org/project/django/) `3.2.8` | 63660 | 47.72 | 52.51 | 48.64

</details>

## Conclusion

Nothing here, just some measures for you.

## License

Licensed under a MIT license (See LICENSE file)