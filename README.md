Arbiter
=======

Arbiter is a connection proxy for PostgreSQL, intended to simplify operations of streaming
replication setups.

[![Build Status](https://secure.travis-ci.org/solvip/arbiter.png)](http://travis-ci.org/solvip/arbiter)

When you start Arbiter, it listens on the `primary` and `follower` addresses defined in arbiter.cfg.
Connections to the `primary` will always be routed to the backend that has the primary role.
Connections to the `follower` will be routed to the closest backend, measured by latency, regardless if that backend is a primary or a follower.

You should configure your application to connect to the `primary` if it performs destructive operations.  Connections to the `follower` should only be used for queries.

# Configuration example

```ini
[main]
;; Connections to the primary address will get routed to
;; the primary backend.  Suitable for INSERT/UPDATE/DELETEs.
primary = 127.0.0.1:5433

;; Connections to the follower address will get routed to
;; any backend.  Suitable for SELECTs.
follower = 127.0.0.1:5434

;; Backends is a comma seperated list of backend servers.
backends = pg1:5432, pg2:5432

[health]
;; The username and password pair describe a PostgreSQL user that has SELECT permissions.
;; Used to query the status of the backends.
username = arbiter
password = arbiter
database = repmgr
```
