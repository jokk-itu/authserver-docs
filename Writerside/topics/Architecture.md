# Architecture

Software Architecture of AuthServer.

## Configuration

Configuring AuthServer is done using the Options pattern.


## DiscoveryDocument

Used to expose metadata about AuthServer.


## JwksDocument

Used to expose the jwks of AuthServer.


## UserInteraction

Used to redirect to pages for login and logout.


## Requests

Endpoints are designed using Minimal APIs.
Each endpoint is implemented by ```IEndpointHandler```, and passes the request to a handler,
and returns a correct HTTP response.

The HTTP request is parsed through an implementation of ```IRequestAccessor```.

The request is handled by an implementation of ```RequestHandler```,
which has the responsibility of Metrics, Transactions and passing the request through validation and processing.

Request validation is handled by an implementation of ```IRequestValidator```.
Request processing is handled by an implementation of ```IRequestProcessor```.


## Infrastructure

### Metrics

Open Telemetry is support through traces, metrics and logging.

Tracing is done in the ```RequestHandler```.
Metrics are executed in client authentication, token handling, interaction and client registration.


### Transactions

Transactions are handled in ```UnitOfWork```.


### Caching

Clients are cached in a DistributedCache, to increase performance.
In all endpoints the client is queried, and rarely updated.
If the client is updated through Registration or JWKS, then it is deleted from the cache,
and will be inserted at first retrieval from the database.