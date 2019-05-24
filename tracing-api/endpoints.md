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

{% api-method-parameter name="request\[method\]" type="string" required=true %}
HTTP method.  
Supports custom methods.
{% endapi-method-parameter %}

{% api-method-parameter name="request\[uri\]" type="string" required=true %}
The request URI.  
e.g.: `http://api.foo.bar/baz`
{% endapi-method-parameter %}

{% api-method-parameter name="request\[ip\]" type="string" required=true %}
Requester's IP address.  
Supports `IPv4` and `IPv6`.
{% endapi-method-parameter %}

{% api-method-parameter name="request\[headers\]" type="object" required=true %}
HTTP request headers.  
Expects `string` keys and `string` or `null` values.
{% endapi-method-parameter %}

{% api-method-parameter name="request\[body\]" type="string" required=true %}
The request body `string`. Allows `null`.
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

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}
This a chain of requests automatically captured using @deep-trace/express-agent. 
{% endapi-method-response-example-description %}

```http
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8

{
  "id": "5500077a-1da3-4f73-9561-d5cbdf127728",
  "parentid": null,
  "rootid": "5500077a-1da3-4f73-9561-d5cbdf127728",
  "request": {
    "ip": "172.20.0.4",
    "method": "GET",
    "uri": "http://api-1.sample.127.0.0.1.nip.io/",
    "headers": {
      "host": "api-1.sample.127.0.0.1.nip.io",
      "connection": "close",
      "x-real-ip": "172.20.0.1",
      "x-forwarded-for": "172.20.0.1",
      "x-forwarded-proto": "http",
      "x-forwarded-ssl": "off",
      "x-forwarded-port": "80",
      "user-agent": "curl/7.54.0",
      "accept": "*/*"
    },
    "body": null,
    "timestamp": "2019-01-02T21:25:34.847Z"
  },
  "response": {
    "status": 200,
    "headers": {
      "x-dns-prefetch-control": "off",
      "x-frame-options": "SAMEORIGIN",
      "strict-transport-security": "max-age=15552000; includeSubDomains",
      "x-download-options": "noopen",
      "x-content-type-options": "nosniff",
      "x-xss-protection": "1; mode=block",
      "deeptrace-id": "5500077a-1da3-4f73-9561-d5cbdf127728",
      "content-type": "application/json; charset=utf-8",
      "content-length": "26",
      "etag": "W/\"1a-30it95O9nVQ9C1pA+b1E6Ng9BKg\""
    },
    "body": "{\"message\":\"hello world!\"}",
    "timestamp": "2019-01-02T21:25:34.982Z"
  },
  "tags": {
    "service": "sample-app-1",
    "release": "v1.3.0",
    "commit": "f3c835b06bc8fc58e414a3ac5730df05448bdf81",
    "platform": "linux",
    "arch": "x64",
    "agent": "@deeptrace/agent@v2.0.0",
    "engine": "node/v10.15.3; v8/v7.0.276.38-node.18"
  },
  "children": [
    {
      "id": "f8b0b722-669a-435d-8d15-bbcb7f0ceabb",
      "parentid": "5500077a-1da3-4f73-9561-d5cbdf127728",
      "rootid": "5500077a-1da3-4f73-9561-d5cbdf127728",
      "request": {
        "ip": "172.20.0.9",
        "method": "GET",
        "uri": "http://sample-app-6:3000/",
        "headers": {
          "accept": "application/json, text/plain, */*",
          "deeptrace-parent-id": "5500077a-1da3-4f73-9561-d5cbdf127728",
          "deeptrace-root-id": "5500077a-1da3-4f73-9561-d5cbdf127728",
          "user-agent": "axios/0.18.0",
          "host": "sample-app-6:3000",
          "connection": "close"
        },
        "body": null,
        "timestamp": "2019-01-02T21:25:34.860Z"
      },
      "response": {
        "status": 200,
        "headers": {
          "x-dns-prefetch-control": "off",
          "x-frame-options": "SAMEORIGIN",
          "strict-transport-security": "max-age=15552000; includeSubDomains",
          "x-download-options": "noopen",
          "x-content-type-options": "nosniff",
          "x-xss-protection": "1; mode=block",
          "deeptrace-id": "f8b0b722-669a-435d-8d15-bbcb7f0ceabb",
          "content-type": "application/json; charset=utf-8",
          "content-length": "26",
          "etag": "W/\"1a-30it95O9nVQ9C1pA+b1E6Ng9BKg\""
        },
        "body": "{\"message\":\"hello world!\"}",
        "timestamp": "2019-01-02T21:25:34.860Z"
      },
      "tags": {
        "service": "sample-app-6",
        "release": "v1.3.0",
        "commit": "f3c835b06bc8fc58e414a3ac5730df05448bdf81",
        "platform": "linux",
        "arch": "x64",
        "agent": "@deeptrace/agent@v2.0.0",
        "engine": "node/v10.15.3; v8/v7.0.276.38-node.18"
      },
      "children": [],
      "took": 0,
      "timestamp": "2019-01-02T21:25:34.882Z"
    },
    {
      "id": "332c84d3-275e-43a6-984d-0538727beef5",
      "parentid": "5500077a-1da3-4f73-9561-d5cbdf127728",
      "rootid": "5500077a-1da3-4f73-9561-d5cbdf127728",
      "request": {
        "ip": "172.20.0.9",
        "method": "GET",
        "uri": "http://sample-app-4:3000/",
        "headers": {
          "accept": "application/json, text/plain, */*",
          "deeptrace-parent-id": "5500077a-1da3-4f73-9561-d5cbdf127728",
          "deeptrace-root-id": "5500077a-1da3-4f73-9561-d5cbdf127728",
          "user-agent": "axios/0.18.0",
          "host": "sample-app-4:3000",
          "connection": "close"
        },
        "body": null,
        "timestamp": "2019-01-02T21:25:34.865Z"
      },
      "response": {
        "status": 200,
        "headers": {
          "x-dns-prefetch-control": "off",
          "x-frame-options": "SAMEORIGIN",
          "strict-transport-security": "max-age=15552000; includeSubDomains",
          "x-download-options": "noopen",
          "x-content-type-options": "nosniff",
          "x-xss-protection": "1; mode=block",
          "deeptrace-id": "332c84d3-275e-43a6-984d-0538727beef5",
          "content-type": "application/json; charset=utf-8",
          "content-length": "26",
          "etag": "W/\"1a-30it95O9nVQ9C1pA+b1E6Ng9BKg\""
        },
        "body": "{\"message\":\"hello world!\"}",
        "timestamp": "2019-01-02T21:25:34.885Z"
      },
      "tags": {
        "service": null,
        "release": "v1.3.0",
        "commit": "f3c835b06bc8fc58e414a3ac5730df05448bdf81",
        "platform": "linux",
        "arch": "x64",
        "agent": "@deeptrace/agent@v2.0.0",
        "engine": "node/v10.15.3; v8/v7.0.276.38-node.18"
      },
      "children": [
        {
          "id": "27a1e416-e200-426a-83aa-fe9eb6e4bafe",
          "parentid": "332c84d3-275e-43a6-984d-0538727beef5",
          "rootid": "5500077a-1da3-4f73-9561-d5cbdf127728",
          "request": {
            "ip": "172.20.0.8",
            "method": "GET",
            "uri": "http://sample-app-6:3000/",
            "headers": {
              "accept": "application/json, text/plain, */*",
              "deeptrace-parent-id": "332c84d3-275e-43a6-984d-0538727beef5",
              "deeptrace-root-id": "5500077a-1da3-4f73-9561-d5cbdf127728",
              "user-agent": "axios/0.18.0",
              "host": "sample-app-6:3000",
              "connection": "close"
            },
            "body": null,
            "timestamp": "2019-01-02T21:25:34.879Z"
          },
          "response": {
            "status": 200,
            "headers": {
              "x-dns-prefetch-control": "off",
              "x-frame-options": "SAMEORIGIN",
              "strict-transport-security": "max-age=15552000; includeSubDomains",
              "x-download-options": "noopen",
              "x-content-type-options": "nosniff",
              "x-xss-protection": "1; mode=block",
              "deeptrace-id": "27a1e416-e200-426a-83aa-fe9eb6e4bafe",
              "content-type": "application/json; charset=utf-8",
              "content-length": "26",
              "etag": "W/\"1a-30it95O9nVQ9C1pA+b1E6Ng9BKg\""
            },
            "body": "{\"message\":\"hello world!\"}",
            "timestamp": "2019-01-02T21:25:34.880Z"
          },
          "tags": {
            "service": "sample-app-6",
            "release": "v1.3.0",
            "commit": "f3c835b06bc8fc58e414a3ac5730df05448bdf81",
            "platform": "linux",
            "arch": "x64",
            "agent": "@deeptrace/agent@v2.0.0",
            "engine": "node/v10.15.3; v8/v7.0.276.38-node.18"
          },
          "children": [],
          "took": 1,
          "timestamp": "2019-01-02T21:25:34.898Z"
        }
      ],
      "took": 20,
      "timestamp": "2019-01-02T21:25:34.950Z"
    },
    {
      "id": "6c691550-ba99-4a39-9172-bf22944e7a8f",
      "parentid": "5500077a-1da3-4f73-9561-d5cbdf127728",
      "rootid": "5500077a-1da3-4f73-9561-d5cbdf127728",
      "request": {
        "ip": "172.20.0.9",
        "method": "GET",
        "uri": "http://sample-app-3:3000/",
        "headers": {
          "accept": "application/json, text/plain, */*",
          "deeptrace-parent-id": "5500077a-1da3-4f73-9561-d5cbdf127728",
          "deeptrace-root-id": "5500077a-1da3-4f73-9561-d5cbdf127728",
          "user-agent": "axios/0.18.0",
          "host": "sample-app-3:3000",
          "connection": "close"
        },
        "body": null,
        "timestamp": "2019-01-02T21:25:34.862Z"
      },
      "response": {
        "status": 200,
        "headers": {
          "x-dns-prefetch-control": "off",
          "x-frame-options": "SAMEORIGIN",
          "strict-transport-security": "max-age=15552000; includeSubDomains",
          "x-download-options": "noopen",
          "x-content-type-options": "nosniff",
          "x-xss-protection": "1; mode=block",
          "deeptrace-id": "6c691550-ba99-4a39-9172-bf22944e7a8f",
          "content-type": "application/json; charset=utf-8",
          "content-length": "26",
          "etag": "W/\"1a-30it95O9nVQ9C1pA+b1E6Ng9BKg\""
        },
        "body": "{\"message\":\"hello world!\"}",
        "timestamp": "2019-01-02T21:25:34.937Z"
      },
      "tags": {
        "service": "sample-app-3",
        "release": "v1.3.0",
        "commit": "f3c835b06bc8fc58e414a3ac5730df05448bdf81",
        "platform": "linux",
        "arch": "x64",
        "agent": "@deeptrace/agent@v2.0.0",
        "engine": "node/v10.15.3; v8/v7.0.276.38-node.18"
      },
      "children": [
        {
          "id": "700c67ad-c40c-4dc6-938e-5acb67ffb818",
          "parentid": "6c691550-ba99-4a39-9172-bf22944e7a8f",
          "rootid": "5500077a-1da3-4f73-9561-d5cbdf127728",
          "request": {
            "ip": "172.20.0.5",
            "method": "GET",
            "uri": "http://sample-app-4:3000/",
            "headers": {
              "accept": "application/json, text/plain, */*",
              "deeptrace-parent-id": "6c691550-ba99-4a39-9172-bf22944e7a8f",
              "deeptrace-root-id": "5500077a-1da3-4f73-9561-d5cbdf127728",
              "user-agent": "axios/0.18.0",
              "host": "sample-app-4:3000",
              "connection": "close"
            },
            "body": null,
            "timestamp": "2019-01-02T21:25:34.903Z"
          },
          "response": {
            "status": 200,
            "headers": {
              "x-dns-prefetch-control": "off",
              "x-frame-options": "SAMEORIGIN",
              "strict-transport-security": "max-age=15552000; includeSubDomains",
              "x-download-options": "noopen",
              "x-content-type-options": "nosniff",
              "x-xss-protection": "1; mode=block",
              "deeptrace-id": "700c67ad-c40c-4dc6-938e-5acb67ffb818",
              "content-type": "application/json; charset=utf-8",
              "content-length": "26",
              "etag": "W/\"1a-30it95O9nVQ9C1pA+b1E6Ng9BKg\""
            },
            "body": "{\"message\":\"hello world!\"}",
            "timestamp": "2019-01-02T21:25:34.935Z"
          },
          "tags": {
            "service": null,
            "release": "v1.3.0",
            "commit": "f3c835b06bc8fc58e414a3ac5730df05448bdf81",
            "platform": "linux",
            "arch": "x64",
            "agent": "@deeptrace/agent@v2.0.0",
            "engine": "node/v10.15.3; v8/v7.0.276.38-node.18"
          },
          "children": [
            {
              "id": "85fe07fc-4756-4ee5-87ec-512ee6c06d62",
              "parentid": "700c67ad-c40c-4dc6-938e-5acb67ffb818",
              "rootid": "5500077a-1da3-4f73-9561-d5cbdf127728",
              "request": {
                "ip": "172.20.0.8",
                "method": "GET",
                "uri": "http://sample-app-6:3000/",
                "headers": {
                  "accept": "application/json, text/plain, */*",
                  "deeptrace-parent-id": "700c67ad-c40c-4dc6-938e-5acb67ffb818",
                  "deeptrace-root-id": "5500077a-1da3-4f73-9561-d5cbdf127728",
                  "user-agent": "axios/0.18.0",
                  "host": "sample-app-6:3000",
                  "connection": "close"
                },
                "body": null,
                "timestamp": "2019-01-02T21:25:34.932Z"
              },
              "response": {
                "status": 200,
                "headers": {
                  "x-dns-prefetch-control": "off",
                  "x-frame-options": "SAMEORIGIN",
                  "strict-transport-security": "max-age=15552000; includeSubDomains",
                  "x-download-options": "noopen",
                  "x-content-type-options": "nosniff",
                  "x-xss-protection": "1; mode=block",
                  "deeptrace-id": "85fe07fc-4756-4ee5-87ec-512ee6c06d62",
                  "content-type": "application/json; charset=utf-8",
                  "content-length": "26",
                  "etag": "W/\"1a-30it95O9nVQ9C1pA+b1E6Ng9BKg\""
                },
                "body": "{\"message\":\"hello world!\"}",
                "timestamp": "2019-01-02T21:25:34.932Z"
              },
              "tags": {
                "service": "sample-app-6",
                "release": "v1.3.0",
                "commit": "f3c835b06bc8fc58e414a3ac5730df05448bdf81",
                "platform": "linux",
                "arch": "x64",
                "agent": "@deeptrace/agent@v2.0.0",
                "engine": "node/v10.15.3; v8/v7.0.276.38-node.18"
              },
              "children": [],
              "took": 0,
              "timestamp": "2019-01-02T21:25:34.953Z"
            }
          ],
          "took": 32,
          "timestamp": "2019-01-02T21:25:35.017Z"
        }
      ],
      "took": 75,
      "timestamp": "2019-01-02T21:25:34.958Z"
    },
    {
      "id": "8bc16cd5-b6b0-46ad-90f5-5c15720b2c0a",
      "parentid": "5500077a-1da3-4f73-9561-d5cbdf127728",
      "rootid": "5500077a-1da3-4f73-9561-d5cbdf127728",
      "request": {
        "ip": "172.20.0.9",
        "method": "GET",
        "uri": "http://sample-app-3:3000/",
        "headers": {
          "accept": "application/json, text/plain, */*",
          "deeptrace-parent-id": "5500077a-1da3-4f73-9561-d5cbdf127728",
          "deeptrace-root-id": "5500077a-1da3-4f73-9561-d5cbdf127728",
          "user-agent": "axios/0.18.0",
          "host": "sample-app-3:3000",
          "connection": "close"
        },
        "body": null,
        "timestamp": "2019-01-02T21:25:34.875Z"
      },
      "response": {
        "status": 200,
        "headers": {
          "x-dns-prefetch-control": "off",
          "x-frame-options": "SAMEORIGIN",
          "strict-transport-security": "max-age=15552000; includeSubDomains",
          "x-download-options": "noopen",
          "x-content-type-options": "nosniff",
          "x-xss-protection": "1; mode=block",
          "deeptrace-id": "8bc16cd5-b6b0-46ad-90f5-5c15720b2c0a",
          "content-type": "application/json; charset=utf-8",
          "content-length": "26",
          "etag": "W/\"1a-30it95O9nVQ9C1pA+b1E6Ng9BKg\""
        },
        "body": "{\"message\":\"hello world!\"}",
        "timestamp": "2019-01-02T21:25:34.946Z"
      },
      "tags": {
        "service": "sample-app-3",
        "release": "v1.3.0",
        "commit": "f3c835b06bc8fc58e414a3ac5730df05448bdf81",
        "platform": "linux",
        "arch": "x64",
        "agent": "@deeptrace/agent@v2.0.0",
        "engine": "node/v10.15.3; v8/v7.0.276.38-node.18"
      },
      "children": [
        {
          "id": "d9904995-7235-4cd3-ab47-b1aea1e84a09",
          "parentid": "8bc16cd5-b6b0-46ad-90f5-5c15720b2c0a",
          "rootid": "5500077a-1da3-4f73-9561-d5cbdf127728",
          "request": {
            "ip": "172.20.0.5",
            "method": "GET",
            "uri": "http://sample-app-4:3000/",
            "headers": {
              "accept": "application/json, text/plain, */*",
              "deeptrace-parent-id": "8bc16cd5-b6b0-46ad-90f5-5c15720b2c0a",
              "deeptrace-root-id": "5500077a-1da3-4f73-9561-d5cbdf127728",
              "user-agent": "axios/0.18.0",
              "host": "sample-app-4:3000",
              "connection": "close"
            },
            "body": null,
            "timestamp": "2019-01-02T21:25:34.924Z"
          },
          "response": {
            "status": 200,
            "headers": {
              "x-dns-prefetch-control": "off",
              "x-frame-options": "SAMEORIGIN",
              "strict-transport-security": "max-age=15552000; includeSubDomains",
              "x-download-options": "noopen",
              "x-content-type-options": "nosniff",
              "x-xss-protection": "1; mode=block",
              "deeptrace-id": "d9904995-7235-4cd3-ab47-b1aea1e84a09",
              "content-type": "application/json; charset=utf-8",
              "content-length": "26",
              "etag": "W/\"1a-30it95O9nVQ9C1pA+b1E6Ng9BKg\""
            },
            "body": "{\"message\":\"hello world!\"}",
            "timestamp": "2019-01-02T21:25:34.944Z"
          },
          "tags": {
            "service": null,
            "release": "v1.3.0",
            "commit": "f3c835b06bc8fc58e414a3ac5730df05448bdf81",
            "platform": "linux",
            "arch": "x64",
            "agent": "@deeptrace/agent@v2.0.0",
            "engine": "node/v10.15.3; v8/v7.0.276.38-node.18"
          },
          "children": [
            {
              "id": "4f989197-2e77-4db4-94be-dba238f641cb",
              "parentid": "d9904995-7235-4cd3-ab47-b1aea1e84a09",
              "rootid": "5500077a-1da3-4f73-9561-d5cbdf127728",
              "request": {
                "ip": "172.20.0.8",
                "method": "GET",
                "uri": "http://sample-app-6:3000/",
                "headers": {
                  "accept": "application/json, text/plain, */*",
                  "deeptrace-parent-id": "d9904995-7235-4cd3-ab47-b1aea1e84a09",
                  "deeptrace-root-id": "5500077a-1da3-4f73-9561-d5cbdf127728",
                  "user-agent": "axios/0.18.0",
                  "host": "sample-app-6:3000",
                  "connection": "close"
                },
                "body": null,
                "timestamp": "2019-01-02T21:25:34.943Z"
              },
              "response": {
                "status": 200,
                "headers": {
                  "x-dns-prefetch-control": "off",
                  "x-frame-options": "SAMEORIGIN",
                  "strict-transport-security": "max-age=15552000; includeSubDomains",
                  "x-download-options": "noopen",
                  "x-content-type-options": "nosniff",
                  "x-xss-protection": "1; mode=block",
                  "deeptrace-id": "4f989197-2e77-4db4-94be-dba238f641cb",
                  "content-type": "application/json; charset=utf-8",
                  "content-length": "26",
                  "etag": "W/\"1a-30it95O9nVQ9C1pA+b1E6Ng9BKg\""
                },
                "body": "{\"message\":\"hello world!\"}",
                "timestamp": "2019-01-02T21:25:34.943Z"
              },
              "tags": {
                "service": "sample-app-6",
                "release": "v1.3.0",
                "commit": "f3c835b06bc8fc58e414a3ac5730df05448bdf81",
                "platform": "linux",
                "arch": "x64",
                "agent": "@deeptrace/agent@v2.0.0",
                "engine": "node/v10.15.3; v8/v7.0.276.38-node.18"
              },
              "children": [],
              "took": 0,
              "timestamp": "2019-01-02T21:25:34.955Z"
            }
          ],
          "took": 20,
          "timestamp": "2019-01-02T21:25:35.031Z"
        }
      ],
      "took": 71,
      "timestamp": "2019-01-02T21:25:34.974Z"
    },
    {
      "id": "7762f277-6484-49ca-bb8a-1b04a39e4384",
      "parentid": "5500077a-1da3-4f73-9561-d5cbdf127728",
      "rootid": "5500077a-1da3-4f73-9561-d5cbdf127728",
      "request": {
        "ip": "172.20.0.9",
        "method": "GET",
        "uri": "http://sample-app-2:3000/",
        "headers": {
          "accept": "application/json, text/plain, */*",
          "deeptrace-parent-id": "5500077a-1da3-4f73-9561-d5cbdf127728",
          "deeptrace-root-id": "5500077a-1da3-4f73-9561-d5cbdf127728",
          "user-agent": "axios/0.18.0",
          "host": "sample-app-2:3000",
          "connection": "close"
        },
        "body": null,
        "timestamp": "2019-01-02T21:25:34.860Z"
      },
      "response": {
        "status": 200,
        "headers": {
          "x-dns-prefetch-control": "off",
          "x-frame-options": "SAMEORIGIN",
          "strict-transport-security": "max-age=15552000; includeSubDomains",
          "x-download-options": "noopen",
          "x-content-type-options": "nosniff",
          "x-xss-protection": "1; mode=block",
          "deeptrace-id": "7762f277-6484-49ca-bb8a-1b04a39e4384",
          "content-type": "application/json; charset=utf-8",
          "content-length": "26",
          "etag": "W/\"1a-30it95O9nVQ9C1pA+b1E6Ng9BKg\""
        },
        "body": "{\"message\":\"hello world!\"}",
        "timestamp": "2019-01-02T21:25:34.974Z"
      },
      "tags": {
        "service": "sample-app-2",
        "release": "v1.3.0",
        "commit": "f3c835b06bc8fc58e414a3ac5730df05448bdf81",
        "platform": "linux",
        "arch": "x64",
        "agent": "@deeptrace/agent@v2.0.0",
        "engine": "node/v10.15.3; v8/v7.0.276.38-node.18"
      },
      "children": [
        {
          "id": "d1729ea7-2be6-40c5-8b2d-9128c9f69c62",
          "parentid": "7762f277-6484-49ca-bb8a-1b04a39e4384",
          "rootid": "5500077a-1da3-4f73-9561-d5cbdf127728",
          "request": {
            "ip": "172.20.0.10",
            "method": "GET",
            "uri": "http://sample-app-5:3000/",
            "headers": {
              "accept": "application/json, text/plain, */*",
              "deeptrace-parent-id": "7762f277-6484-49ca-bb8a-1b04a39e4384",
              "deeptrace-root-id": "5500077a-1da3-4f73-9561-d5cbdf127728",
              "user-agent": "axios/0.18.0",
              "host": "sample-app-5:3000",
              "connection": "close"
            },
            "body": null,
            "timestamp": "2019-01-02T21:25:34.894Z"
          },
          "response": {
            "status": 200,
            "headers": {
              "x-dns-prefetch-control": "off",
              "x-frame-options": "SAMEORIGIN",
              "strict-transport-security": "max-age=15552000; includeSubDomains",
              "x-download-options": "noopen",
              "x-content-type-options": "nosniff",
              "x-xss-protection": "1; mode=block",
              "deeptrace-id": "d1729ea7-2be6-40c5-8b2d-9128c9f69c62",
              "content-type": "application/json; charset=utf-8",
              "content-length": "26",
              "etag": "W/\"1a-30it95O9nVQ9C1pA+b1E6Ng9BKg\""
            },
            "body": "{\"message\":\"hello world!\"}",
            "timestamp": "2019-01-02T21:25:34.896Z"
          },
          "tags": {
            "service": "sample-app-5",
            "release": "v1.3.0",
            "commit": "f3c835b06bc8fc58e414a3ac5730df05448bdf81",
            "platform": "linux",
            "arch": "x64",
            "agent": "@deeptrace/agent@v2.0.0",
            "engine": "node/v10.15.3; v8/v7.0.276.38-node.18"
          },
          "children": [],
          "took": 2,
          "timestamp": "2019-01-02T21:25:34.910Z"
        },
        {
          "id": "1602a5ba-c5e8-4693-ad5d-9f4b793ea6db",
          "parentid": "7762f277-6484-49ca-bb8a-1b04a39e4384",
          "rootid": "5500077a-1da3-4f73-9561-d5cbdf127728",
          "request": {
            "ip": "172.20.0.10",
            "method": "GET",
            "uri": "http://sample-app-3:3000/",
            "headers": {
              "accept": "application/json, text/plain, */*",
              "deeptrace-parent-id": "7762f277-6484-49ca-bb8a-1b04a39e4384",
              "deeptrace-root-id": "5500077a-1da3-4f73-9561-d5cbdf127728",
              "user-agent": "axios/0.18.0",
              "host": "sample-app-3:3000",
              "connection": "close"
            },
            "body": null,
            "timestamp": "2019-01-02T21:25:34.907Z"
          },
          "response": {
            "status": 200,
            "headers": {
              "x-dns-prefetch-control": "off",
              "x-frame-options": "SAMEORIGIN",
              "strict-transport-security": "max-age=15552000; includeSubDomains",
              "x-download-options": "noopen",
              "x-content-type-options": "nosniff",
              "x-xss-protection": "1; mode=block",
              "deeptrace-id": "1602a5ba-c5e8-4693-ad5d-9f4b793ea6db",
              "content-type": "application/json; charset=utf-8",
              "content-length": "26",
              "etag": "W/\"1a-30it95O9nVQ9C1pA+b1E6Ng9BKg\""
            },
            "body": "{\"message\":\"hello world!\"}",
            "timestamp": "2019-01-02T21:25:34.972Z"
          },
          "tags": {
            "service": "sample-app-3",
            "release": "v1.3.0",
            "commit": "f3c835b06bc8fc58e414a3ac5730df05448bdf81",
            "platform": "linux",
            "arch": "x64",
            "agent": "@deeptrace/agent@v2.0.0",
            "engine": "node/v10.15.3; v8/v7.0.276.38-node.18"
          },
          "children": [
            {
              "id": "ff701c5a-3096-4eec-9207-3abfdea25fa2",
              "parentid": "1602a5ba-c5e8-4693-ad5d-9f4b793ea6db",
              "rootid": "5500077a-1da3-4f73-9561-d5cbdf127728",
              "request": {
                "ip": "172.20.0.5",
                "method": "GET",
                "uri": "http://sample-app-4:3000/",
                "headers": {
                  "accept": "application/json, text/plain, */*",
                  "deeptrace-parent-id": "1602a5ba-c5e8-4693-ad5d-9f4b793ea6db",
                  "deeptrace-root-id": "5500077a-1da3-4f73-9561-d5cbdf127728",
                  "user-agent": "axios/0.18.0",
                  "host": "sample-app-4:3000",
                  "connection": "close"
                },
                "body": null,
                "timestamp": "2019-01-02T21:25:34.932Z"
              },
              "response": {
                "status": 200,
                "headers": {
                  "x-dns-prefetch-control": "off",
                  "x-frame-options": "SAMEORIGIN",
                  "strict-transport-security": "max-age=15552000; includeSubDomains",
                  "x-download-options": "noopen",
                  "x-content-type-options": "nosniff",
                  "x-xss-protection": "1; mode=block",
                  "deeptrace-id": "ff701c5a-3096-4eec-9207-3abfdea25fa2",
                  "content-type": "application/json; charset=utf-8",
                  "content-length": "26",
                  "etag": "W/\"1a-30it95O9nVQ9C1pA+b1E6Ng9BKg\""
                },
                "body": "{\"message\":\"hello world!\"}",
                "timestamp": "2019-01-02T21:25:34.970Z"
              },
              "tags": {
                "service": null,
                "release": "v1.3.0",
                "commit": "f3c835b06bc8fc58e414a3ac5730df05448bdf81",
                "platform": "linux",
                "arch": "x64",
                "agent": "@deeptrace/agent@v2.0.0",
                "engine": "node/v10.15.3; v8/v7.0.276.38-node.18"
              },
              "children": [
                {
                  "id": "28708f23-51d8-4b3f-9d0f-5dc4231d782c",
                  "parentid": "ff701c5a-3096-4eec-9207-3abfdea25fa2",
                  "rootid": "5500077a-1da3-4f73-9561-d5cbdf127728",
                  "request": {
                    "ip": "172.20.0.8",
                    "method": "GET",
                    "uri": "http://sample-app-6:3000/",
                    "headers": {
                      "accept": "application/json, text/plain, */*",
                      "deeptrace-parent-id": "ff701c5a-3096-4eec-9207-3abfdea25fa2",
                      "deeptrace-root-id": "5500077a-1da3-4f73-9561-d5cbdf127728",
                      "user-agent": "axios/0.18.0",
                      "host": "sample-app-6:3000",
                      "connection": "close"
                    },
                    "body": null,
                    "timestamp": "2019-01-02T21:25:34.968Z"
                  },
                  "response": {
                    "status": 200,
                    "headers": {
                      "x-dns-prefetch-control": "off",
                      "x-frame-options": "SAMEORIGIN",
                      "strict-transport-security": "max-age=15552000; includeSubDomains",
                      "x-download-options": "noopen",
                      "x-content-type-options": "nosniff",
                      "x-xss-protection": "1; mode=block",
                      "deeptrace-id": "28708f23-51d8-4b3f-9d0f-5dc4231d782c",
                      "content-type": "application/json; charset=utf-8",
                      "content-length": "26",
                      "etag": "W/\"1a-30it95O9nVQ9C1pA+b1E6Ng9BKg\""
                    },
                    "body": "{\"message\":\"hello world!\"}",
                    "timestamp": "2019-01-02T21:25:34.968Z"
                  },
                  "tags": {
                    "service": "sample-app-6",
                    "release": "v1.3.0",
                    "commit": "f3c835b06bc8fc58e414a3ac5730df05448bdf81",
                    "platform": "linux",
                    "arch": "x64",
                    "agent": "@deeptrace/agent@v2.0.0",
                    "engine": "node/v10.15.3; v8/v7.0.276.38-node.18"
                  },
                  "children": [],
                  "took": 0,
                  "timestamp": "2019-01-02T21:25:35.036Z"
                }
              ],
              "took": 38,
              "timestamp": "2019-01-02T21:25:35.067Z"
            }
          ],
          "took": 65,
          "timestamp": "2019-01-02T21:25:35.050Z"
        },
        {
          "id": "1a444d89-aa57-4c1f-ae60-d90fc26f40f9",
          "parentid": "7762f277-6484-49ca-bb8a-1b04a39e4384",
          "rootid": "5500077a-1da3-4f73-9561-d5cbdf127728",
          "request": {
            "ip": "172.20.0.10",
            "method": "GET",
            "uri": "http://sample-app-4:3000/",
            "headers": {
              "accept": "application/json, text/plain, */*",
              "deeptrace-parent-id": "7762f277-6484-49ca-bb8a-1b04a39e4384",
              "deeptrace-root-id": "5500077a-1da3-4f73-9561-d5cbdf127728",
              "user-agent": "axios/0.18.0",
              "host": "sample-app-4:3000",
              "connection": "close"
            },
            "body": null,
            "timestamp": "2019-01-02T21:25:34.928Z"
          },
          "response": {
            "status": 200,
            "headers": {
              "x-dns-prefetch-control": "off",
              "x-frame-options": "SAMEORIGIN",
              "strict-transport-security": "max-age=15552000; includeSubDomains",
              "x-download-options": "noopen",
              "x-content-type-options": "nosniff",
              "x-xss-protection": "1; mode=block",
              "deeptrace-id": "1a444d89-aa57-4c1f-ae60-d90fc26f40f9",
              "content-type": "application/json; charset=utf-8",
              "content-length": "26",
              "etag": "W/\"1a-30it95O9nVQ9C1pA+b1E6Ng9BKg\""
            },
            "body": "{\"message\":\"hello world!\"}",
            "timestamp": "2019-01-02T21:25:34.966Z"
          },
          "tags": {
            "service": null,
            "release": "v1.3.0",
            "commit": "f3c835b06bc8fc58e414a3ac5730df05448bdf81",
            "platform": "linux",
            "arch": "x64",
            "agent": "@deeptrace/agent@v2.0.0",
            "engine": "node/v10.15.3; v8/v7.0.276.38-node.18"
          },
          "children": [
            {
              "id": "c425d55e-3854-4004-90d8-2b575fd3dd65",
              "parentid": "1a444d89-aa57-4c1f-ae60-d90fc26f40f9",
              "rootid": "5500077a-1da3-4f73-9561-d5cbdf127728",
              "request": {
                "ip": "172.20.0.8",
                "method": "GET",
                "uri": "http://sample-app-6:3000/",
                "headers": {
                  "accept": "application/json, text/plain, */*",
                  "deeptrace-parent-id": "1a444d89-aa57-4c1f-ae60-d90fc26f40f9",
                  "deeptrace-root-id": "5500077a-1da3-4f73-9561-d5cbdf127728",
                  "user-agent": "axios/0.18.0",
                  "host": "sample-app-6:3000",
                  "connection": "close"
                },
                "body": null,
                "timestamp": "2019-01-02T21:25:34.957Z"
              },
              "response": {
                "status": 200,
                "headers": {
                  "x-dns-prefetch-control": "off",
                  "x-frame-options": "SAMEORIGIN",
                  "strict-transport-security": "max-age=15552000; includeSubDomains",
                  "x-download-options": "noopen",
                  "x-content-type-options": "nosniff",
                  "x-xss-protection": "1; mode=block",
                  "deeptrace-id": "c425d55e-3854-4004-90d8-2b575fd3dd65",
                  "content-type": "application/json; charset=utf-8",
                  "content-length": "26",
                  "etag": "W/\"1a-30it95O9nVQ9C1pA+b1E6Ng9BKg\""
                },
                "body": "{\"message\":\"hello world!\"}",
                "timestamp": "2019-01-02T21:25:34.958Z"
              },
              "tags": {
                "service": "sample-app-6",
                "release": "v1.3.0",
                "commit": "f3c835b06bc8fc58e414a3ac5730df05448bdf81",
                "platform": "linux",
                "arch": "x64",
                "agent": "@deeptrace/agent@v2.0.0",
                "engine": "node/v10.15.3; v8/v7.0.276.38-node.18"
              },
              "children": [],
              "took": 1,
              "timestamp": "2019-01-02T21:25:35.015Z"
            }
          ],
          "took": 38,
          "timestamp": "2019-01-02T21:25:35.062Z"
        }
      ],
      "took": 114,
      "timestamp": "2019-01-02T21:25:35.021Z"
    }
  ],
  "took": 135,
  "timestamp": "2019-01-02T21:25:35.042Z"
}
```
{% endapi-method-response-example %}

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

