# Generic Erlang CoAP Client/Server

Pure Erlang implementation of the Constrained Application Protocol (CoAP),
which aims to be conformant with:
 - CoAP core protocol [RFC 7252](https://tools.ietf.org/rfc/rfc7252.txt)
 - CoAP Observe option [RFC 7641](https://tools.ietf.org/rfc/rfc7641.txt)
 - Block-wise transfers in CoAP [draft-ietf-core-block-18](https://tools.ietf.org/id/draft-ietf-core-block-18.txt)
 - CoRE link format [RFC 6690](https://tools.ietf.org/rfc/rfc6690.txt)

Tested with the following CoAP implementations:
 - C [libcoap](https://www.libcoap.net/) (develop branch)
 - [coap.me](http://coap.me/)

Used in the following applications:
 - [CoAP Publish-Subscribe interface to RabbitMQ](https://github.com/gotthardp/rabbitmq-coap-pubsub)

The API may change without notice and some functions may not be implemented.
Please [add an Issue](https://github.com/gotthardp/gen_coap/issues/new)
if you find a bug or miss a feature.


## Usage
[![Build Status](https://travis-ci.org/gotthardp/gen_coap.svg?branch=master)](https://travis-ci.org/gotthardp/gen_coap)

### Client
Get a resource by:
```erlang
{ok, content, Data} = coap_client:request(get, "coap://coap.me:5683")
```
No application need to be started.

Then, you can invoke the client and access the server resources:

    $ ./coap-client coap://127.0.0.1/.well-known/core

### Server
The server out of a box does not offer any resources. To offer CoAP access to
some server resources you need to implement the `coap_resource` behaviour,
which defines callbacks that the server invokes upon reception of a CoAP request.
 - `coap_discover` is called when a CoAP client asks for the list of
   ".well-known/core" resources.
 - `coap_get`, `coap_post`, `coap_put` or `coap_delete` is called when the server
   receives a GET, POST, PUT or DELETE request for a resource.
 - `coap_observe` or `coap_unobserve` is called upon a GET request with an
   Observe=0 or Observe=1 option.

You can also start the server from command line:

    $ erl -pa ebin
    1> application:start(gen_coap).

### Design

The architecture looks as follows:
![GitHub Logo](https://rawgit.com/gotthardp/gen_coap/master/doc/architecture.svg)
