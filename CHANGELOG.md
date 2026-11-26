# Changelog

## 1.0.0

**Release date:** TBD

Version 1.0.0 is the first release of `redis`, which runs on Lus 2.0.

- Added the `redis` package.
  - `redis.connect`: Connects to a Redis or Valkey server and returns a client with a pool of connections, which has a method for every command of the server.
  - `redis.is_error`: Returns true for an error object that the package made.
  - `redis.rate_limit_store`: Returns a store for the `rate_limit` package that keeps its counters in Redis.
  - `redis.session_store`: Returns a store for the `session` package that keeps its sessions in Redis.
  - `redis.queue_store`: Returns a store for the `queue` package that keeps its jobs in Redis.
