###### Portable Linked Profiles

# Directory

## What is a PLP directory?

PLP Directories add logic around the profiles. They pull profiles from providers and serve them to Browsers. Possible logic could be validation, authentication, notification, etc...

## Source

The source code can be found at

> https://github.com/hackers4peace/plp-directory

## API

Supports CORS ([Cross-Origin Resource Sharing](http://enable-cors.org/))

We evaluate [Hydra](http://www.hydra-cg.com/) and [LDP](http://www.w3.org/TR/ldp/), for now simple Level-3 REST

### GET /

status: *implementing*

returns all listings in directory

request: *currently expects no parameters, in a future we can
enable filtering by "@type" of listed profile (Person, Organization,
Event, Place, Asset etc.)*

content-type: *application/ld+json* (or not recommended *application/json*)

response: *JSON-LD object with graph including all listings each with embeded profile*

```js
{
  "@context": "http://plp.hackers4peace.net/context.jsonld",
  "@graph": [
    {
      "@id": "http://directory-domain.tld/faa52ac0-c9f5-4b8c-be1c-4480f353315f"
      "@type": "Listing",
      "about": {
        "@id": "http://provider-domain.tld/449b829a-0fbd-420a-bbe4-70d11527d62b",
        "@type": "Person",
        "name": "Alice Wonder",
        ...
      }
    },
    {
      "@id": "http://directory-domain.tld/a43057b8-134f-49a4-991a-bf910f0803e9"
      "@type": "Listing",
      "about": {
        "@id": "http://provider-domain.tld/11055695-8a65-4284-a6e7-ab96884e7658",
        "@type": "Person",
        "name": "Bob Secure",
        ...
      }
    },
    ...
  ]
}
```

### POST /

status: *implementing*

adds listing to directory, only URI needed since directory will fetch
full profile data from provider

request: expects JSON-LD object with profile to list


```js
{
  "@context": "http://plp.hackers4peace.net/context.jsonld",
  "@id": "http://provider-domain.tld/449b829a-0fbd-420a-bbe4-70d11527d62b",
  "@type": "Person"
 }
```

content-type: *application/ld+json* (or not recommended *application/json*)

response: *JSON-LD object with URI of newly created listing based on
directory's domain name and generated [UUID](http://en.wikipedia.org/wiki/Universally_unique_identifier)*

```js
{
  "@context": "http://plp.hackers4peace.net/context.jsonld",
  "@id": "http://directory-domain.tld/faa52ac0-c9f5-4b8c-be1c-4480f353315f",
  "@type": "Listing"
 }
```

### GET /:uuid

status: *planned*

gets single listing

### PUT /:uuid

status: *planned*

requests update of a listing

### DELETE /:uuid

status: *planned*

requests deletion of a listing
