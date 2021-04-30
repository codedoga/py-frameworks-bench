# Async Python Web Frameworks comparison

https://klen.github.io/py-frameworks-bench/
----------
#### Updated: 2021-04-30

[![benchmarks](https://github.com/klen/py-frameworks-bench/actions/workflows/benchmarks.yml/badge.svg)](https://github.com/klen/py-frameworks-bench/actions/workflows/benchmarks.yml)
[![tests](https://github.com/klen/py-frameworks-bench/actions/workflows/tests.yml/badge.svg)](https://github.com/klen/py-frameworks-bench/actions/workflows/tests.yml)

----------

This is a simple benchmark for python async frameworks. Almost all of the
frameworks are ASGI-compatible (aiohttp is an exception).

The objective of the benchmark is not testing deployment (like uvicorn vs
hypercorn and etc) or database (ORM, drivers) but instead test the frameworks
itself. The benchmark checks request parsing (body, headers, formdata,
queries), routing, responses.

## Table of contents

* [The Methodic](#the-methodic)
* [The Results](#the-results-2021-04-30)
    * [Accept a request and return HTML response with a custom dynamic header](#html)
    * [Parse uploaded file, store it on disk and return a text response](#upload)
    * [Parse path params, query string, JSON body and return a json response](#api)
    * [Composite stats ](#composite)

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

2. "Upload" test: accept an uploaded file and store it on disk. The test
   simulates multipart formdata processing and work with files.

3. "API" test: Check headers, parse path params, query string, JSON body and return a json
   response. The test simulates an JSON REST API.


## The Results (2021-04-30)

<h3 id="html"> Accept a request and return HTML response with a custom dynamic header</h3>
<details open>
<summary> The test simulates just a single HTML response. </summary>

Sorted by max req/s

| Framework | Requests/sec | Latency 50% (ms) | Latency 75% (ms) | Latency Avg (ms) |
| --------- | -----------: | ---------------: | ---------------: | ---------------: |
| [blacksheep](https://pypi.org/project/blacksheep/) `1.0.3` | 13805 | 3.71 | 6.29 | 4.61
| [muffin](https://pypi.org/project/muffin/) `0.70` | 12636 | 4.02 | 6.97 | 5.04
| [falcon](https://pypi.org/project/falcon/) `3.0.0` | 11495 | 4.43 | 7.62 | 5.54
| [starlette](https://pypi.org/project/starlette/) `0.14.2` | 10599 | 4.77 | 8.22 | 6.01
| [emmett](https://pypi.org/project/emmett/) `2.2.1` | 9922 | 5.06 | 8.88 | 6.42
| [fastapi](https://pypi.org/project/fastapi/) `0.63.0` | 7262 | 6.89 | 12.15 | 8.80
| [sanic](https://pypi.org/project/sanic/) `21.3.4` | 6360 | 7.89 | 13.76 | 10.05
| [aiohttp](https://pypi.org/project/aiohttp/) `3.7.4.post0` | 5088 | 12.40 | 12.88 | 12.58
| [quart](https://pypi.org/project/quart/) `0.14.1` | 2771 | 21.87 | 25.84 | 23.09
| [django](https://pypi.org/project/django/) `3.2` | 1368 | 45.20 | 52.33 | 46.72


</details>

<h3 id="upload"> Parse uploaded file, store it on disk and return a text response</h3>
<details open>
<summary> The test simulates multipart formdata processing and work with files.  </summary>

Sorted by max req/s

| Framework | Requests/sec | Latency 50% (ms) | Latency 75% (ms) | Latency Avg (ms) |
| --------- | -----------: | ---------------: | ---------------: | ---------------: |
| [blacksheep](https://pypi.org/project/blacksheep/) `1.0.3` | 4160 | 12.11 | 21.48 | 16.12
| [muffin](https://pypi.org/project/muffin/) `0.70` | 3203 | 15.87 | 27.90 | 19.96
| [sanic](https://pypi.org/project/sanic/) `21.3.4` | 3042 | 16.68 | 28.77 | 21.02
| [falcon](https://pypi.org/project/falcon/) `3.0.0` | 2747 | 18.59 | 31.54 | 23.34
| [starlette](https://pypi.org/project/starlette/) `0.14.2` | 1949 | 25.65 | 45.74 | 32.79
| [fastapi](https://pypi.org/project/fastapi/) `0.63.0` | 1793 | 27.77 | 49.93 | 35.64
| [aiohttp](https://pypi.org/project/aiohttp/) `3.7.4.post0` | 1788 | 35.60 | 36.36 | 35.79
| [emmett](https://pypi.org/project/emmett/) `2.2.1` | 1207 | 49.96 | 58.89 | 52.96
| [quart](https://pypi.org/project/quart/) `0.14.1` | 1095 | 58.92 | 61.70 | 58.36
| [django](https://pypi.org/project/django/) `3.2` | 776 | 80.86 | 93.00 | 82.32


</details>

<h3 id="api"> Parse path params, query string, JSON body and return a json response</h3>
<details open>
<summary> The test simulates a simple JSON REST API endpoint.  </summary>

Sorted by max req/s

| Framework | Requests/sec | Latency 50% (ms) | Latency 75% (ms) | Latency Avg (ms) |
| --------- | -----------: | ---------------: | ---------------: | ---------------: |
| [blacksheep](https://pypi.org/project/blacksheep/) `1.0.3` | 7499 | 6.74 | 12.06 | 8.51
| [falcon](https://pypi.org/project/falcon/) `3.0.0` | 7232 | 6.93 | 12.29 | 8.83
| [muffin](https://pypi.org/project/muffin/) `0.70` | 7070 | 7.05 | 12.58 | 9.02
| [starlette](https://pypi.org/project/starlette/) `0.14.2` | 5602 | 8.95 | 15.91 | 11.40
| [sanic](https://pypi.org/project/sanic/) `21.3.4` | 5379 | 9.19 | 16.35 | 11.89
| [emmett](https://pypi.org/project/emmett/) `2.2.1` | 5126 | 9.66 | 17.36 | 12.50
| [fastapi](https://pypi.org/project/fastapi/) `0.63.0` | 4406 | 11.30 | 20.27 | 14.50
| [aiohttp](https://pypi.org/project/aiohttp/) `3.7.4.post0` | 3232 | 19.70 | 20.24 | 19.81
| [quart](https://pypi.org/project/quart/) `0.14.1` | 1767 | 36.32 | 38.11 | 36.19
| [django](https://pypi.org/project/django/) `3.2` | 1146 | 54.00 | 62.70 | 55.80

</details>

<h3 id="composite"> Composite stats </h3>
<details open>
<summary> Combined benchmarks results</summary>

Sorted by completed requests

| Framework | Requests completed | Avg Latency 50% (ms) | Avg Latency 75% (ms) | Avg Latency (ms) |
| --------- | -----------------: | -------------------: | -------------------: | ---------------: |
| [blacksheep](https://pypi.org/project/blacksheep/) `1.0.3` | 381960 | 7.52 | 13.28 | 9.75
| [muffin](https://pypi.org/project/muffin/) `0.70` | 343635 | 8.98 | 15.82 | 11.34
| [falcon](https://pypi.org/project/falcon/) `3.0.0` | 322110 | 9.98 | 17.15 | 12.57
| [starlette](https://pypi.org/project/starlette/) `0.14.2` | 272250 | 13.12 | 23.29 | 16.73
| [emmett](https://pypi.org/project/emmett/) `2.2.1` | 243825 | 21.56 | 28.38 | 23.96
| [sanic](https://pypi.org/project/sanic/) `21.3.4` | 221715 | 11.25 | 19.63 | 14.32
| [fastapi](https://pypi.org/project/fastapi/) `0.63.0` | 201915 | 15.32 | 27.45 | 19.65
| [aiohttp](https://pypi.org/project/aiohttp/) `3.7.4.post0` | 151620 | 22.57 | 23.16 | 22.73
| [quart](https://pypi.org/project/quart/) `0.14.1` | 84495 | 39.04 | 41.88 | 39.21
| [django](https://pypi.org/project/django/) `3.2` | 49350 | 60.02 | 69.34 | 61.61

</details>

## Conclusion

Nothing here, just some measures for you.

## License

Licensed under a MIT license (See LICENSE file)