# Endpoints

{% api-method method="post" host="https://api.deep-trace.io" path="/v1/traces" %}
{% api-method-summary %}
Create a trace
{% endapi-method-summary %}

{% api-method-description %}
Creates a trace document.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-headers %}
{% api-method-parameter name="Contet-Type" type="string" required=true %}
Allowed values: `application/json` or `application/x-www-form-urlencoded`.
{% endapi-method-parameter %}
{% endapi-method-headers %}

{% api-method-body-parameters %}
{% api-method-parameter name="id" type="string" required=true %}
Unique id.  
e.g.: `8d4758f7-a794-48de-a4cf-8ebc559226d6`
{% endapi-method-parameter %}

{% api-method-parameter name="parent\_id" type="string" required=true %}
It's parent trace id.  
e.g.: `5e326978-bc52-4b69-b0af-efd97a538ea0`
{% endapi-method-parameter %}

{% api-method-parameter name="chain\_id" type="string" required=true %}
The chain id to which the trace belongs.  
e.g.: `62a0d7b0-3ed7-4bb8-ac9d-293bf7e614e8`
{% endapi-method-parameter %}

{% api-method-parameter name="request\[url\]\[method\]" type="string" required=true %}
Request HTTP method.  
e.g.: `POST`
{% endapi-method-parameter %}

{% api-method-parameter name="request\[url\]\[full\]" type="string" required=true %}
Request URL.  
e.g.: `https://api.acme.com/v1/foobars`
{% endapi-method-parameter %}

{% api-method-parameter name="request\[headers\]" type="object" required=true %}
Request headers.  
e.g.:
{% endapi-method-parameter %}

{% api-method-parameter name="request\[body\]" type="string" required=true %}
Request body.  
e.g.: `{ "foo": "bar" }`
{% endapi-method-parameter %}

{% api-method-parameter name="response\[status\]\[code\]" type="number" required=true %}
Response code.  
e.g.: `201`
{% endapi-method-parameter %}

{% api-method-parameter name="response\[headers\]" type="object" required=true %}
Response headers.  
e.g.: 
{% endapi-method-parameter %}

{% api-method-parameter name="response\[body\]" type="string" required=true %}
Response body.  
e.g.: `{ "foo": "bar" }`
{% endapi-method-parameter %}

{% api-method-parameter name="requested\_at" type="string" required=true %}
ISO8601 datetime.  
e.g.: `2018-12-27T17:09:00.000-02:00`
{% endapi-method-parameter %}

{% api-method-parameter name="responded\_at" type="string" required=true %}
ISO8601 datetime.  
e.g.: `2018-12-27T17:09:00.000-02:00`
{% endapi-method-parameter %}
{% endapi-method-body-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=202 %}
{% api-method-response-example-description %}
Trace has been queued to be persisted.
{% endapi-method-response-example-description %}

```http
HTTP/1.1 202 Accepted
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
    "code": "DUPLICATED_TRACE_ID",
    "message": "There's a trace with id \"b1ef74d5-959b-46b5-b361-dcece28cf9be\" which was created at \"2018-12-27T16:38:00.083-02:00\"."
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
    "code": "FAILED_SCHEMA_VALIDATION",
    "message": "\"id\" property is required."
  }
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% hint style="warning" %}
I'm conflicted between two REST response status standards, regarding the trace creation:

* **202**: the entity is valid and is yet to be created/persisted;
* **204**: the request was successful but there's no relevant content to be returned.

Though the trace creation response fits in both standars, I'm in favor of using **202** response status as it allows room for persistency failures since this standard explicitly states the entity has not been created/persisted yet.
{% endhint %}

{% api-method method="get" host="https://api.deep-trace.io" path="/v1/traces/:id" %}
{% api-method-summary %}
Get a trace
{% endapi-method-summary %}

{% api-method-description %}
Gets a trace document.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="id" type="string" required=true %}
Unique trace id.
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```http
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8

{
  "id": "b1ef74d5-959b-46b5-b361-dcece28cf9be",
  "parent_id": "e41fcfb0-2d42-4ac2-a011-c4974936e8d2",
  "chain_id": "c04f2a05-514e-4ce6-9685-c53811e9f5bc",
  "request": {
    "method": "POST",
    "url": {
      "full": "http://localhost:3000/baz",
      "host": "http://localhost:3000",
      "path": "/baz",
      "query": {}
    },
    "headers": {
      "Accept": "application/json",
      "Authorization": "[REDACTED]",
      "Content-Type": "application/json"
    },
    "body": "{ \"foo\": \"bar\" }"
  },
  "response": {
    "status": {
      "code": 204,
      "name": "No Content" 
    },
    "headers": {},
    "body": ""
  },
  "requested_at": "2018-12-27T16:38:00.000-02:00",
  "responded_at": "2018-12-27T16:38:00.073-02:00",
  "elapsed_time": 73,
  "created_at": "2018-12-27T16:38:00.083-02:00"
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
    "message": "Requested trace was not found."
  }
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

