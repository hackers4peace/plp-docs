###### [Portable Linked Profiles](http://hackers4peace.net/plp)

# Provider

## About

PLP Providers store profiles. They interact with PLP-Directories, serving them profiles, and with PLP-Editors, which create/update/delete the profiles stored on them.

## Source

The source code can be found at

> https://github.com/hackers4peace/plp-provider

## API

Supports CORS ([Cross-Origin Resource Sharing](http://enable-cors.org/))

*We evaluate [Hydra](http://www.hydra-cg.com/) and [LDP](http://www.w3.org/TR/ldp/)*


### GET /:uuid

status: *basic implementation*

gets single profile

#### request

* content-type: **application/ld+json**

#### response

* code: **200 OK**
* payload: *JSON-LD object with full profile*

```js
{
  "@context": "http://plp.hackers4peace.net/context.jsonld",
  "@id": "http://provider-domain.tld/449b829a-0fbd-420a-bbe4-70d11527d62b",
  "@type": "Person",
  "name": "Alice Wonder",
  "memberOf": [
    {
      "@id": "http://wl.tld",
      "@type": "Organization",
      "name": "Wonderlanderians"
    },
    ...
  ],
  ...
}
```

* errors
 * *HTTP 301 Moved Permanently* - if profile moved elsewhere
 * *HTTP 404 Not Found* - if profile never existed
 * *HTTP 410 Gone* - if profile existed but got deleted
 * *HTTP 500 Internal Server Error*

### POST /

status: *basic implementation*

creates new profile

#### request

* content-type: **application/ld+json**
* headers
 * *Authorization: Bearer [token]*
* payload: *profile data* ([PLP
Editor](https://github.com/hackers4peace/plp-editor) can
generate it)


```js
{
  "@context": "http://plp.hackers4peace.net/context.jsonld",
  "@type": "Person",
  "name": "Alice Wonderland",
  "memberOf": [
    {
      "@id": "http://wl.tld",
      "@type": "Organization",
      "name": "Wonderlanderians"
    },
    ...
  ],
  ...
 }
```


#### response

* code: **201 Created**
* content-type: **application/ld+json**
* payload: *JSON-LD object with URI of newly created profile based on
provider's domain name and generated [UUID](http://en.wikipedia.org/wiki/Universally_unique_identifier)*

```js
{
 "@context": "http://plp.hackers4peace.net/context.jsonld",
 "@id": "http://provider-domain.tld/449b829a-0fbd-420a-bbe4-70d11527d62b",
 "@type" "Person"
}
```

* errors
 * *HTTP 401 Unauthorized* - if authentication fails
 * *HTTP 409 Conflict* - if payload includes ```@id```
 * *HTTP 500 Internal Server Error*

### PUT /:uuid

status: *basic implementation*

updates profile

#### request

* content-type: **application/ld+json**
* headers
 * *Authorization: Bearer [token]*
* payload: *same as POST but requires payload to have an @id matching one of stored profiles stored on this provider*

#### response

* code: **200 OK**
* errors
 * *HTTP 401 Unauthorized* - if authentication fails
 * *HTTP 403 Forbidden* - if authorization fails
 * *HTTP 400 Bad Request* - if payload includes ```@id``` different then requested URI
 * *HTTP 500 Internal Server Error*

### DELETE /:uuid

status: *basic implementation*

deletes profile

#### request

* headers
 * *Authorization: Bearer [token]*

#### response

* code: *204 No Content*
* errors
 * *HTTP 401 Unauthorized* - if authentication fails
 * *HTTP 403 Forbidden* - if authorization fails
 * *HTTP 500 Internal Server Error*


## Authentication

Currently we use
[Mozilla Persona](https://developer.mozilla.org/en-US/Persona) and
[JSON Web Token (JWT)](http://jwt.io/)

### POST /auth/login

status: *basic implementation*

#### request

* content-type: **application/json**
* payload: [Mozilla Persona assertion](https://developer.mozilla.org/en-US/docs/Web/API/navigator.id.get)

```json
{ "assertion": "[assertion]" }
```

#### response

* content-type: **application/json**
* payload: [JSON Web Token (JWT)](http://jwt.io)

```json
{ "token": "[token]" }
```

### POST /auth/logout

status: *planned*

