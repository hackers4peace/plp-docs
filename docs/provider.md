###### Portable Linked Profiles

# Provider

## About

PLP Providers store profiles. They interact with PLP-Directories, serving them profiles, and with PLP-Editors, which create/update/delete the profiles stored on them.

## Source

The source code can be found at

> https://github.com/hackers4peace/plp-converters

## API

Supports CORS ([Cross-Origin Resource Sharing](http://enable-cors.org/))

We evaluate [Hydra](http://www.hydra-cg.com/) and [LDP](http://www.w3.org/TR/ldp/), for now simple Level-3 REST

### POST /

status: *basic implementation*

if payload includes ```@id``` returns *HTTP 409 Conflict*, otherwise creates new profile

request: *expects JSON-LD object with profile data* ([PLP
Editor](https://github.com/hackers4peace/plp-editor) can
generate them)


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

content-type: *application/ld+json*

response: *JSON-LD object with URI of newly created profile based on
provider's domain name and generated [UUID](http://en.wikipedia.org/wiki/Universally_unique_identifier)*

```js
{
 "@context": "http://plp.hackers4peace.net/context.jsonld",
 "@id": "http://provider-domain.tld/449b829a-0fbd-420a-bbe4-70d11527d62b",
 "@type" "Person"
}
```

### GET /:uuid

status: *basic implementation*

gets single profile

request: *currently expects no parameters*

content-type: *application/ld+json*

response: *JSON-LD object with full profile*

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

### PUT /:uuid

status: *basic implementation*

updates profile

same as POST but requires payload to have an @id matching one of the
profiles stored on this provider


### DELETE /:uuid

status: *basic implementation*

deletes profile

request: *currently expects no parameters*
response: 200 OK
