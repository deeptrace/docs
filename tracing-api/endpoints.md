# Endpoints

{% api-method method="post" host="https://api.deep-trace.io" path="/traces" %}
{% api-method-summary %}
Ingest a trace
{% endapi-method-summary %}

{% api-method-description %}
Ingests a trace.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-headers %}
{% api-method-parameter name="Contet-Type" type="string" required=true %}
Allowed values: `application/json` or `application/x-www-form-urlencoded`.
{% endapi-method-parameter %}
{% endapi-method-headers %}

{% api-method-form-data-parameters %}
{% api-method-parameter name="id" type="string" required=true %}
Trace id \(a.k.a request id\) which is an `UUID/v4`.
e.g.: `5500077a-1da3-4f73-9561-d5cbdf127728`
{% endapi-method-parameter %}

{% api-method-parameter name="parentid" type="string" required=true %}
Parent trace id which is an `UUID/v4`.
e.g.: `5500077a-1da3-4f73-9561-d5cbdf127728`
{% endapi-method-parameter %}

{% api-method-parameter name="rootid" type="string" required=true %}
Root trace id, which is an `UUID/v4`.
e.g.: `5500077a-1da3-4f73-9561-d5cbdf127728`
{% endapi-method-parameter %}

{% api-method-parameter name="tags" type="object" required=true %}
Custom tags used for aggregated statistics.
Expects `string` keys and `string` values.
{% endapi-method-parameter %}

{% api-method-parameter name="caller\[ip\]" type="string" required=true %}
Requester's IP address.
Supports `IPv4` and `IPv6`.
{% endapi-method-parameter %}

{% api-method-parameter name="request\[method\]" type="string" required=true %}
HTTP method.
Supports custom methods.
{% endapi-method-parameter %}

{% api-method-parameter name="request\[url\]" type="string" required=true %}
Request's URL.
e.g.: `http://api.foo.bar/baz`
{% endapi-method-parameter %}

{% api-method-parameter name="request\[headers\]" type="object" required=true %}
Request's headers.
Expects `string` keys and `string` or `null` values.
{% endapi-method-parameter %}

{% api-method-parameter name="request\[query\]" type="string" required=true %}
Request's query string as `string`. Allows `null`.
e.g.: `?foo=bar`
{% endapi-method-parameter %}

{% api-method-parameter name="request\[body\]" type="string" required=true %}
Request's body as `string`. Allows `null`.
{% endapi-method-parameter %}

{% api-method-parameter name="request\[timestamp\]" type="string" required=true %}
ISO8601 datetime string.
e.g.: `2019-01-02T21:25:35.042Z`
{% endapi-method-parameter %}

{% api-method-parameter name="response\[status\]" type="number" required=true %}
HTTP response status.
Supports custom status.
{% endapi-method-parameter %}

{% api-method-parameter name="response\[headers\]" type="object" required=true %}
HTTP response headers.
Expects `string` keys and `string` values.
{% endapi-method-parameter %}

{% api-method-parameter name="response\[body\]" type="string" required=true %}
HTTP response body `string`. Allows `null`.
{% endapi-method-parameter %}

{% api-method-parameter name="response\[timestamp\]" type="string" required=true %}
ISO8601 datetime string.
e.g.: `2019-01-02T21:25:35.042Z`
{% endapi-method-parameter %}

{% api-method-parameter name="timestamp" type="string" required=true %}
ISO8601 datetime string.
e.g.: `2019-01-02T21:25:35.042Z`
{% endapi-method-parameter %}
{% endapi-method-form-data-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=204 %}
{% api-method-response-example-description %}
Trace has been recorded.
{% endapi-method-response-example-description %}

```http
HTTP/1.1 204 No Content
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=409 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```http
HTTP/1.1 409 Conflict
Content-Type: application/json

{
  "status" 409,
  "error": {
    "code": "DUPLICATED_TRACE",
    "message": "There's another trace with the same id \"b1ef74d5-959b-46b5-b361-dcece28cf9be\". Did you know that a sample of 3.26*10^16 UUIDs has a 99.99% chance of not having any duplicates?"
  }
}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=422 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```http
HTTP/1.1 422 Unprocessable Entity
Content-Type: application/json

{
  "status" 422,
  "error": {
    "code": "FAILED_PAYLOAD_VALIDATION",
    "message": "\"id\" property is required."
  }
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% api-method method="get" host="https://api.deep-trace.io" path="/traces/:id" %}
{% api-method-summary %}
Get a trace
{% endapi-method-summary %}

{% api-method-description %}
Gets a trace and its nested children traces.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="id" type="string" required=true %}
Trace id \(a.k.a request id\) which is an `UUID/v4`.
e.g.: `5500077a-1da3-4f73-9561-d5cbdf127728`
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}
{% endapi-method-request %}

{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}
This is a chain of requests captured using [@deeptrace/agent](https://deeptrace.gitbook.io/docs/js-packages/deeptrace-agent).
{% endapi-method-response-example-description %}

```http
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
Content-Length: 16222

{
  "id": "c27c5789-bc5a-4a82-bafc-d896d044cedc",
  "parentid": null,
  "rootid": "c27c5789-bc5a-4a82-bafc-d896d044cedc",
  "tags": {
    "app": "express",
    "environment": "development",
    "commit": "9ecaf8355e696128d4f19102c5c29feb96de4042",
    "release": "v1.0.0",
    "arch": "x64",
    "platform": "darwin",
    "engine": "node/12.3.1; v8/7.4.288.27-node.18",
    "agent": "@deeptrace/agent@1.0.24"
  },
  "caller": {
    "ip": "::1"
  },
  "request": {
    "method": "POST",
    "url": "http://localhost:3001/express",
    "query": "?foo=bar",
    "headers": {
      "host": "localhost:3001",
      "user-agent": "curl/7.54.0",
      "accept": "*/*",
      "content-type": "application/json",
      "content-length": "49"
    },
    "body": "{\"name\":\"Rafael\",\"email\":\"me@rwillians.com\"}",
    "timestamp": "2019-06-07T03:34:46.420Z"
  },
  "response": {
    "status": 200,
    "headers": {
      "x-powered-by": "Express",
      "deeptrace-request-id": "c27c5789-bc5a-4a82-bafc-d896d044cedc",
      "content-type": "application/json"
    },
    "body": "{\"message\":\"hello from express\"}",
    "timestamp": "2019-06-07T03:34:46.475Z"
  },
  "timestamp": "2019-06-07T03:34:46.501Z",
  "children": [
    {
      "id": "995f5eb3-8770-44bf-9cc3-814cc69c8d0e",
      "parentid": "c27c5789-bc5a-4a82-bafc-d896d044cedc",
      "rootid": "c27c5789-bc5a-4a82-bafc-d896d044cedc",
      "tags": {
        "app": "koa",
        "environment": "development",
        "commit": "9ecaf8355e696128d4f19102c5c29feb96de4042",
        "release": "v1.0.0",
        "arch": "x64",
        "platform": "darwin",
        "engine": "node/12.3.1; v8/7.4.288.27-node.18",
        "agent": "@deeptrace/agent@1.0.24"
      },
      "caller": {
        "ip": "::ffff:127.0.0.1"
      },
      "request": {
        "method": "GET",
        "url": "http://localhost:3001/koa",
        "query": null,
        "headers": {
          "accept": "application/json, text/plain, */*",
          "user-agent": "axios/0.19.0",
          "deeptrace-parent-request-id": "c27c5789-bc5a-4a82-bafc-d896d044cedc",
          "deeptrace-root-request-id": "c27c5789-bc5a-4a82-bafc-d896d044cedc",
          "host": "localhost:3001",
          "connection": "close"
        },
        "body": null,
        "timestamp": "2019-06-07T03:34:46.436Z"
      },
      "response": {
        "status": 200,
        "headers": {
          "deeptrace-request-id": "995f5eb3-8770-44bf-9cc3-814cc69c8d0e",
          "content-type": "application/json"
        },
        "body": "{\"message\":\"hello from koa\"}",
        "timestamp": "2019-06-07T03:34:46.462Z"
      },
      "timestamp": "2019-06-07T03:34:46.494Z",
      "children": [
        {
          "id": "bd2560a2-c235-4dcb-a954-e09d12d2cc67",
          "parentid": "995f5eb3-8770-44bf-9cc3-814cc69c8d0e",
          "rootid": "c27c5789-bc5a-4a82-bafc-d896d044cedc",
          "tags": {
            "app": "native",
            "environment": "development",
            "commit": "9ecaf8355e696128d4f19102c5c29feb96de4042",
            "release": "v1.0.0",
            "arch": "x64",
            "platform": "darwin",
            "engine": "node/12.3.1; v8/7.4.288.27-node.18",
            "agent": "@deeptrace/agent@1.0.24"
          },
          "caller": {
            "ip": "::ffff:127.0.0.1"
          },
          "request": {
            "method": "GET",
            "url": "http://localhost:3001/native",
            "query": null,
            "headers": {
              "accept": "application/json, text/plain, */*",
              "user-agent": "axios/0.19.0",
              "deeptrace-parent-request-id": "995f5eb3-8770-44bf-9cc3-814cc69c8d0e",
              "deeptrace-root-request-id": "c27c5789-bc5a-4a82-bafc-d896d044cedc",
              "host": "localhost:3001",
              "connection": "close"
            },
            "body": null,
            "timestamp": "2019-06-07T03:34:46.442Z"
          },
          "response": {
            "status": 200,
            "headers": {
              "deeptrace-request-id": "bd2560a2-c235-4dcb-a954-e09d12d2cc67",
              "content-type": "application/json"
            },
            "body": "{\"message\":\"hello from native\"}",
            "timestamp": "2019-06-07T03:34:46.443Z"
          },
          "timestamp": "2019-06-07T03:34:46.470Z",
          "children": [],
          "took": 1
        },
        {
          "id": "52432fe2-c81c-4339-b8f9-98f8780334a4",
          "parentid": "995f5eb3-8770-44bf-9cc3-814cc69c8d0e",
          "rootid": "c27c5789-bc5a-4a82-bafc-d896d044cedc",
          "tags": {
            "app": "native",
            "environment": "development",
            "commit": "9ecaf8355e696128d4f19102c5c29feb96de4042",
            "release": "v1.0.0",
            "arch": "x64",
            "platform": "darwin",
            "engine": "node/12.3.1; v8/7.4.288.27-node.18",
            "agent": "@deeptrace/agent@1.0.24"
          },
          "caller": {
            "ip": "::ffff:127.0.0.1"
          },
          "request": {
            "method": "GET",
            "url": "http://localhost:3001/native",
            "query": null,
            "headers": {
              "accept": "application/json, text/plain, */*",
              "user-agent": "axios/0.19.0",
              "deeptrace-parent-request-id": "995f5eb3-8770-44bf-9cc3-814cc69c8d0e",
              "deeptrace-root-request-id": "c27c5789-bc5a-4a82-bafc-d896d044cedc",
              "host": "localhost:3001",
              "connection": "close"
            },
            "body": null,
            "timestamp": "2019-06-07T03:34:46.451Z"
          },
          "response": {
            "status": 200,
            "headers": {
              "deeptrace-request-id": "52432fe2-c81c-4339-b8f9-98f8780334a4",
              "content-type": "application/json"
            },
            "body": "{\"message\":\"hello from native\"}",
            "timestamp": "2019-06-07T03:34:46.451Z"
          },
          "timestamp": "2019-06-07T03:34:46.474Z",
          "children": [],
          "took": 0
        },
        {
          "id": "d56177ca-a13d-4dec-b682-6afe464cbfa7",
          "parentid": "995f5eb3-8770-44bf-9cc3-814cc69c8d0e",
          "rootid": "c27c5789-bc5a-4a82-bafc-d896d044cedc",
          "tags": {
            "app": "native",
            "environment": "development",
            "commit": "9ecaf8355e696128d4f19102c5c29feb96de4042",
            "release": "v1.0.0",
            "arch": "x64",
            "platform": "darwin",
            "engine": "node/12.3.1; v8/7.4.288.27-node.18",
            "agent": "@deeptrace/agent@1.0.24"
          },
          "caller": {
            "ip": "::ffff:127.0.0.1"
          },
          "request": {
            "method": "GET",
            "url": "http://localhost:3001/native",
            "query": null,
            "headers": {
              "accept": "application/json, text/plain, */*",
              "user-agent": "axios/0.19.0",
              "deeptrace-parent-request-id": "995f5eb3-8770-44bf-9cc3-814cc69c8d0e",
              "deeptrace-root-request-id": "c27c5789-bc5a-4a82-bafc-d896d044cedc",
              "host": "localhost:3001",
              "connection": "close"
            },
            "body": null,
            "timestamp": "2019-06-07T03:34:46.452Z"
          },
          "response": {
            "status": 200,
            "headers": {
              "deeptrace-request-id": "d56177ca-a13d-4dec-b682-6afe464cbfa7",
              "content-type": "application/json"
            },
            "body": "{\"message\":\"hello from native\"}",
            "timestamp": "2019-06-07T03:34:46.452Z"
          },
          "timestamp": "2019-06-07T03:34:46.476Z",
          "children": [],
          "took": 0
        },
        {
          "id": "ebf01084-1a69-4bc9-8867-7f7fb79c3865",
          "parentid": "995f5eb3-8770-44bf-9cc3-814cc69c8d0e",
          "rootid": "c27c5789-bc5a-4a82-bafc-d896d044cedc",
          "tags": {
            "app": "native",
            "environment": "development",
            "commit": "9ecaf8355e696128d4f19102c5c29feb96de4042",
            "release": "v1.0.0",
            "arch": "x64",
            "platform": "darwin",
            "engine": "node/12.3.1; v8/7.4.288.27-node.18",
            "agent": "@deeptrace/agent@1.0.24"
          },
          "caller": {
            "ip": "::ffff:127.0.0.1"
          },
          "request": {
            "method": "GET",
            "url": "http://localhost:3001/native",
            "query": null,
            "headers": {
              "accept": "application/json, text/plain, */*",
              "user-agent": "axios/0.19.0",
              "deeptrace-parent-request-id": "995f5eb3-8770-44bf-9cc3-814cc69c8d0e",
              "deeptrace-root-request-id": "c27c5789-bc5a-4a82-bafc-d896d044cedc",
              "host": "localhost:3001",
              "connection": "close"
            },
            "body": null,
            "timestamp": "2019-06-07T03:34:46.453Z"
          },
          "response": {
            "status": 200,
            "headers": {
              "deeptrace-request-id": "ebf01084-1a69-4bc9-8867-7f7fb79c3865",
              "content-type": "application/json"
            },
            "body": "{\"message\":\"hello from native\"}",
            "timestamp": "2019-06-07T03:34:46.453Z"
          },
          "timestamp": "2019-06-07T03:34:46.478Z",
          "children": [],
          "took": 0
        },
        {
          "id": "d6f2c315-cf18-49de-9bb1-3d7b13fcc94f",
          "parentid": "995f5eb3-8770-44bf-9cc3-814cc69c8d0e",
          "rootid": "c27c5789-bc5a-4a82-bafc-d896d044cedc",
          "tags": {
            "app": "native",
            "environment": "development",
            "commit": "9ecaf8355e696128d4f19102c5c29feb96de4042",
            "release": "v1.0.0",
            "arch": "x64",
            "platform": "darwin",
            "engine": "node/12.3.1; v8/7.4.288.27-node.18",
            "agent": "@deeptrace/agent@1.0.24"
          },
          "caller": {
            "ip": "::ffff:127.0.0.1"
          },
          "request": {
            "method": "GET",
            "url": "http://localhost:3001/native",
            "query": null,
            "headers": {
              "accept": "application/json, text/plain, */*",
              "user-agent": "axios/0.19.0",
              "deeptrace-parent-request-id": "995f5eb3-8770-44bf-9cc3-814cc69c8d0e",
              "deeptrace-root-request-id": "c27c5789-bc5a-4a82-bafc-d896d044cedc",
              "host": "localhost:3001",
              "connection": "close"
            },
            "body": null,
            "timestamp": "2019-06-07T03:34:46.454Z"
          },
          "response": {
            "status": 200,
            "headers": {
              "deeptrace-request-id": "d6f2c315-cf18-49de-9bb1-3d7b13fcc94f",
              "content-type": "application/json"
            },
            "body": "{\"message\":\"hello from native\"}",
            "timestamp": "2019-06-07T03:34:46.455Z"
          },
          "timestamp": "2019-06-07T03:34:46.480Z",
          "children": [],
          "took": 1
        },
        {
          "id": "91e3dc2e-1ce2-4257-be66-c378828353c0",
          "parentid": "995f5eb3-8770-44bf-9cc3-814cc69c8d0e",
          "rootid": "c27c5789-bc5a-4a82-bafc-d896d044cedc",
          "tags": {
            "app": "native",
            "environment": "development",
            "commit": "9ecaf8355e696128d4f19102c5c29feb96de4042",
            "release": "v1.0.0",
            "arch": "x64",
            "platform": "darwin",
            "engine": "node/12.3.1; v8/7.4.288.27-node.18",
            "agent": "@deeptrace/agent@1.0.24"
          },
          "caller": {
            "ip": "::ffff:127.0.0.1"
          },
          "request": {
            "method": "GET",
            "url": "http://localhost:3001/native",
            "query": null,
            "headers": {
              "accept": "application/json, text/plain, */*",
              "user-agent": "axios/0.19.0",
              "deeptrace-parent-request-id": "995f5eb3-8770-44bf-9cc3-814cc69c8d0e",
              "deeptrace-root-request-id": "c27c5789-bc5a-4a82-bafc-d896d044cedc",
              "host": "localhost:3001",
              "connection": "close"
            },
            "body": null,
            "timestamp": "2019-06-07T03:34:46.456Z"
          },
          "response": {
            "status": 200,
            "headers": {
              "deeptrace-request-id": "91e3dc2e-1ce2-4257-be66-c378828353c0",
              "content-type": "application/json"
            },
            "body": "{\"message\":\"hello from native\"}",
            "timestamp": "2019-06-07T03:34:46.456Z"
          },
          "timestamp": "2019-06-07T03:34:46.481Z",
          "children": [],
          "took": 0
        }
      ],
      "took": 26
    },
    {
      "id": "60a2d941-d24d-4df9-b120-cdf37f718156",
      "parentid": "c27c5789-bc5a-4a82-bafc-d896d044cedc",
      "rootid": "c27c5789-bc5a-4a82-bafc-d896d044cedc",
      "tags": {
        "app": "koa",
        "environment": "development",
        "commit": "9ecaf8355e696128d4f19102c5c29feb96de4042",
        "release": "v1.0.0",
        "arch": "x64",
        "platform": "darwin",
        "engine": "node/12.3.1; v8/7.4.288.27-node.18",
        "agent": "@deeptrace/agent@1.0.24"
      },
      "caller": {
        "ip": "::ffff:127.0.0.1"
      },
      "request": {
        "method": "GET",
        "url": "http://localhost:3001/koa",
        "query": null,
        "headers": {
          "accept": "application/json, text/plain, */*",
          "user-agent": "axios/0.19.0",
          "deeptrace-parent-request-id": "c27c5789-bc5a-4a82-bafc-d896d044cedc",
          "deeptrace-root-request-id": "c27c5789-bc5a-4a82-bafc-d896d044cedc",
          "host": "localhost:3001",
          "connection": "close"
        },
        "body": null,
        "timestamp": "2019-06-07T03:34:46.438Z"
      },
      "response": {
        "status": 200,
        "headers": {
          "deeptrace-request-id": "60a2d941-d24d-4df9-b120-cdf37f718156",
          "content-type": "application/json"
        },
        "body": "{\"message\":\"hello from koa\"}",
        "timestamp": "2019-06-07T03:34:46.473Z"
      },
      "timestamp": "2019-06-07T03:34:46.500Z",
      "children": [
        {
          "id": "9d423fae-076b-45fd-8da9-d1ddf89e3584",
          "parentid": "60a2d941-d24d-4df9-b120-cdf37f718156",
          "rootid": "c27c5789-bc5a-4a82-bafc-d896d044cedc",
          "tags": {
            "app": "native",
            "environment": "development",
            "commit": "9ecaf8355e696128d4f19102c5c29feb96de4042",
            "release": "v1.0.0",
            "arch": "x64",
            "platform": "darwin",
            "engine": "node/12.3.1; v8/7.4.288.27-node.18",
            "agent": "@deeptrace/agent@1.0.24"
          },
          "caller": {
            "ip": "::ffff:127.0.0.1"
          },
          "request": {
            "method": "GET",
            "url": "http://localhost:3001/native",
            "query": null,
            "headers": {
              "accept": "application/json, text/plain, */*",
              "user-agent": "axios/0.19.0",
              "deeptrace-parent-request-id": "60a2d941-d24d-4df9-b120-cdf37f718156",
              "deeptrace-root-request-id": "c27c5789-bc5a-4a82-bafc-d896d044cedc",
              "host": "localhost:3001",
              "connection": "close"
            },
            "body": null,
            "timestamp": "2019-06-07T03:34:46.457Z"
          },
          "response": {
            "status": 200,
            "headers": {
              "deeptrace-request-id": "9d423fae-076b-45fd-8da9-d1ddf89e3584",
              "content-type": "application/json"
            },
            "body": "{\"message\":\"hello from native\"}",
            "timestamp": "2019-06-07T03:34:46.457Z"
          },
          "timestamp": "2019-06-07T03:34:46.482Z",
          "children": [],
          "took": 0
        },
        {
          "id": "fa17ddad-6b4f-4e8b-824c-c9545d54e3d1",
          "parentid": "60a2d941-d24d-4df9-b120-cdf37f718156",
          "rootid": "c27c5789-bc5a-4a82-bafc-d896d044cedc",
          "tags": {
            "app": "native",
            "environment": "development",
            "commit": "9ecaf8355e696128d4f19102c5c29feb96de4042",
            "release": "v1.0.0",
            "arch": "x64",
            "platform": "darwin",
            "engine": "node/12.3.1; v8/7.4.288.27-node.18",
            "agent": "@deeptrace/agent@1.0.24"
          },
          "caller": {
            "ip": "::ffff:127.0.0.1"
          },
          "request": {
            "method": "GET",
            "url": "http://localhost:3001/native",
            "query": null,
            "headers": {
              "accept": "application/json, text/plain, */*",
              "user-agent": "axios/0.19.0",
              "deeptrace-parent-request-id": "60a2d941-d24d-4df9-b120-cdf37f718156",
              "deeptrace-root-request-id": "c27c5789-bc5a-4a82-bafc-d896d044cedc",
              "host": "localhost:3001",
              "connection": "close"
            },
            "body": null,
            "timestamp": "2019-06-07T03:34:46.458Z"
          },
          "response": {
            "status": 200,
            "headers": {
              "deeptrace-request-id": "fa17ddad-6b4f-4e8b-824c-c9545d54e3d1",
              "content-type": "application/json"
            },
            "body": "{\"message\":\"hello from native\"}",
            "timestamp": "2019-06-07T03:34:46.458Z"
          },
          "timestamp": "2019-06-07T03:34:46.497Z",
          "children": [],
          "took": 0
        },
        {
          "id": "d37a19bd-8c21-4c62-ad49-67637f74a26a",
          "parentid": "60a2d941-d24d-4df9-b120-cdf37f718156",
          "rootid": "c27c5789-bc5a-4a82-bafc-d896d044cedc",
          "tags": {
            "app": "native",
            "environment": "development",
            "commit": "9ecaf8355e696128d4f19102c5c29feb96de4042",
            "release": "v1.0.0",
            "arch": "x64",
            "platform": "darwin",
            "engine": "node/12.3.1; v8/7.4.288.27-node.18",
            "agent": "@deeptrace/agent@1.0.24"
          },
          "caller": {
            "ip": "::ffff:127.0.0.1"
          },
          "request": {
            "method": "GET",
            "url": "http://localhost:3001/native",
            "query": null,
            "headers": {
              "accept": "application/json, text/plain, */*",
              "user-agent": "axios/0.19.0",
              "deeptrace-parent-request-id": "60a2d941-d24d-4df9-b120-cdf37f718156",
              "deeptrace-root-request-id": "c27c5789-bc5a-4a82-bafc-d896d044cedc",
              "host": "localhost:3001",
              "connection": "close"
            },
            "body": null,
            "timestamp": "2019-06-07T03:34:46.465Z"
          },
          "response": {
            "status": 200,
            "headers": {
              "deeptrace-request-id": "d37a19bd-8c21-4c62-ad49-67637f74a26a",
              "content-type": "application/json"
            },
            "body": "{\"message\":\"hello from native\"}",
            "timestamp": "2019-06-07T03:34:46.465Z"
          },
          "timestamp": "2019-06-07T03:34:46.499Z",
          "children": [],
          "took": 0
        },
        {
          "id": "ffb19f5f-20ad-473d-a959-2e6186634f7a",
          "parentid": "60a2d941-d24d-4df9-b120-cdf37f718156",
          "rootid": "c27c5789-bc5a-4a82-bafc-d896d044cedc",
          "tags": {
            "app": "native",
            "environment": "development",
            "commit": "9ecaf8355e696128d4f19102c5c29feb96de4042",
            "release": "v1.0.0",
            "arch": "x64",
            "platform": "darwin",
            "engine": "node/12.3.1; v8/7.4.288.27-node.18",
            "agent": "@deeptrace/agent@1.0.24"
          },
          "caller": {
            "ip": "::ffff:127.0.0.1"
          },
          "request": {
            "method": "GET",
            "url": "http://localhost:3001/native",
            "query": null,
            "headers": {
              "accept": "application/json, text/plain, */*",
              "user-agent": "axios/0.19.0",
              "deeptrace-parent-request-id": "60a2d941-d24d-4df9-b120-cdf37f718156",
              "deeptrace-root-request-id": "c27c5789-bc5a-4a82-bafc-d896d044cedc",
              "host": "localhost:3001",
              "connection": "close"
            },
            "body": null,
            "timestamp": "2019-06-07T03:34:46.464Z"
          },
          "response": {
            "status": 200,
            "headers": {
              "deeptrace-request-id": "ffb19f5f-20ad-473d-a959-2e6186634f7a",
              "content-type": "application/json"
            },
            "body": "{\"message\":\"hello from native\"}",
            "timestamp": "2019-06-07T03:34:46.464Z"
          },
          "timestamp": "2019-06-07T03:34:46.496Z",
          "children": [],
          "took": 0
        },
        {
          "id": "9cde8205-7496-4957-b540-f032e7b8d1ab",
          "parentid": "60a2d941-d24d-4df9-b120-cdf37f718156",
          "rootid": "c27c5789-bc5a-4a82-bafc-d896d044cedc",
          "tags": {
            "app": "native",
            "environment": "development",
            "commit": "9ecaf8355e696128d4f19102c5c29feb96de4042",
            "release": "v1.0.0",
            "arch": "x64",
            "platform": "darwin",
            "engine": "node/12.3.1; v8/7.4.288.27-node.18",
            "agent": "@deeptrace/agent@1.0.24"
          },
          "caller": {
            "ip": "::ffff:127.0.0.1"
          },
          "request": {
            "method": "GET",
            "url": "http://localhost:3001/native",
            "query": null,
            "headers": {
              "accept": "application/json, text/plain, */*",
              "user-agent": "axios/0.19.0",
              "deeptrace-parent-request-id": "60a2d941-d24d-4df9-b120-cdf37f718156",
              "deeptrace-root-request-id": "c27c5789-bc5a-4a82-bafc-d896d044cedc",
              "host": "localhost:3001",
              "connection": "close"
            },
            "body": null,
            "timestamp": "2019-06-07T03:34:46.470Z"
          },
          "response": {
            "status": 200,
            "headers": {
              "deeptrace-request-id": "9cde8205-7496-4957-b540-f032e7b8d1ab",
              "content-type": "application/json"
            },
            "body": "{\"message\":\"hello from native\"}",
            "timestamp": "2019-06-07T03:34:46.470Z"
          },
          "timestamp": "2019-06-07T03:34:49.496Z",
          "children": [],
          "took": 0
        },
        {
          "id": "966ef9a1-cb04-40c1-8cde-20aa25083d70",
          "parentid": "60a2d941-d24d-4df9-b120-cdf37f718156",
          "rootid": "c27c5789-bc5a-4a82-bafc-d896d044cedc",
          "tags": {
            "app": "native",
            "environment": "development",
            "commit": "9ecaf8355e696128d4f19102c5c29feb96de4042",
            "release": "v1.0.0",
            "arch": "x64",
            "platform": "darwin",
            "engine": "node/12.3.1; v8/7.4.288.27-node.18",
            "agent": "@deeptrace/agent@1.0.24"
          },
          "caller": {
            "ip": "::ffff:127.0.0.1"
          },
          "request": {
            "method": "GET",
            "url": "http://localhost:3001/native",
            "query": null,
            "headers": {
              "accept": "application/json, text/plain, */*",
              "user-agent": "axios/0.19.0",
              "deeptrace-parent-request-id": "60a2d941-d24d-4df9-b120-cdf37f718156",
              "deeptrace-root-request-id": "c27c5789-bc5a-4a82-bafc-d896d044cedc",
              "host": "localhost:3001",
              "connection": "close"
            },
            "body": null,
            "timestamp": "2019-06-07T03:34:46.469Z"
          },
          "response": {
            "status": 200,
            "headers": {
              "deeptrace-request-id": "966ef9a1-cb04-40c1-8cde-20aa25083d70",
              "content-type": "application/json"
            },
            "body": "{\"message\":\"hello from native\"}",
            "timestamp": "2019-06-07T03:34:46.469Z"
          },
          "timestamp": "2019-06-07T03:34:49.498Z",
          "children": [],
          "took": 0
        }
      ],
      "took": 35
    }
  ],
  "took": 55
}
```
{% endapi-method-response-example %}

{% api-method-response %}
{% api-method-response-example httpCode=404 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```http
HTTP/1.1 404 Not Found
Content-Type: application/json; charset=utf-8

{
  "status": 404,
  "error": {
    "code": "TRACE_NOT_FOUND",
    "message": "There seems to be not trace with given id \"b1ef74d5-959b-46b5-b361-dcece28cf9be\". Are you sure that's the right id?"
  }
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

