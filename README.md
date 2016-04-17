# Jsonapi::Resources::Fallback

Extends Jsonapi::Resources to accept and respond with unwrapped JSON payloads when 'application/json' is specified during content negotiation.  

## Description

This gem adds a content-negotiation layer so that Rails applications built with the Jsonapi::Resources gem can be utilized in full by JSONAPI clients, and more simply by generic API clients.  The idea being that if a generic API client can't and never could work with JSONAPI, then let them work via plain, unwrapped JSON.

Assumptions:

* Many clients readily produce/consume unwrapped JSON (ex `curl` + `jq`).  These clients are unlikely to utilize the metadata and hypermedia aspects of the full JSONAPI payload.

* The graph of objects in a JSONAPI payload can be flattened to a tree, provided cycles are broken and you can throw out the metadata/hypermedia.  The reverse is also possible; a tree can be represented as a graph of objects.

As such when the app:

* Receives an `application/json` tree then flatten it into `application/vnd.api+json` graph pre-processing.  No metadata/hypermedia will be present because the clients would not have produced it.

* Is asked to produce `application/json` then serialize the `application/vnd.api+json` graph as a tree.  Metadata/hypermedia is thrown out in this case, which is fine because the clients wouldn't have used it anyway.

This gem adds a serializer to walk the object graph and produce a tree from it for replies, as well as SOMETHING (don't know what yet) to do the reverse on requests. 

## License

The gem is available as open source under the terms of the [MIT License](http://opensource.org/licenses/MIT).

