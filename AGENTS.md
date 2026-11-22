# AGENTS.md

`lus-lang/atmos-redis` is the repository of the `redis` package of the atmosphere registry. Programs load the package with `require("https://atmosp.here/2.0/redis.lus")`.

## Scope

`redis` is a client for Redis and Valkey servers, with pipelines, transactions, publish and subscribe, scripts, scans, and a pool of connections. It also provides the stores of the `rate_limit`, `session`, and `queue` packages. The public API is every function that `redis.lus` returns in its module table and every documented method of the values they return. A change to that API needs a new entry in `CHANGELOG.md` and a version change in the header.

## Layout

- `redis.lus` is the whole package. Its header doc comment contains the package metadata and the declared permissions.
- `tests/*_test.lus` are the tests. They load `redis.lus` from the parent of `TEST_DIR`, the directory that holds the test file.
- `tests/fake_redis.lus` is the server that the tests talk to. It speaks RESP3 and RESP2 on a loopback port and keeps its data in tables. It runs each Lua script of the stores as a Lus function of the same name in `F.scripts`, which it finds by the comment on the first line of the script.

## Dependencies

- the standard library `network` module, for TCP and TLS sockets
- the standard library `crypto` module, for the SHA-1 digest of a script

The tests of the stores load `rate_limit`, `session`, `queue`, `cookie`, and `web` from checkouts of `lus-lang/atmos-rate-limit`, `lus-lang/atmos-session`, `lus-lang/atmos-queue`, `lus-lang/atmos-cookie`, and `lus-lang/atmos-web` next to this repository, so clone them into the same parent directory first.

## Commands

Run these commands from the root of the repository.

```sh
lus test tests          # run the tests
lus check redis.lus     # lint
lus doc redis.lus       # print the reference
```

The tests bind loopback ports, so they need a shell that allows it. Two more tests run against a real server when `LUS_TEST_REDIS` holds its URL, such as `redis://127.0.0.1:6379/15`. They write and delete keys that start with `atmos-test:`.

## Rules

- Every public function has a doc comment that states each result and each error, following `spec/design/style.md` of the Lus repository.
- The package never calls `pledge`. A new permission is declared in the header and in the README.
- A change to a Lua script of a store comes with the same change to its Lus function in `tests/fake_redis.lus` and with a run of the tests against a real server.
- Prose follows the `lus-writing` skill, which `lus agents init` installs and https://lus.dev/agents/skills/lus-writing/SKILL.md serves. In particular, prose has no dashes or semicolons and never names the Roblox Lua dialect.
- Every bug fix comes with a test that fails without it.
