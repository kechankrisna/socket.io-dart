## 2.0.0+1
* fixed on connect 

## 2.0.0

* support dart 3.0.0

**Breaking Changes — Dart 3 upgrade:**

* Raised SDK constraint to `>=3.0.0 <4.0.0`
* Upgraded `stream` to `^4.0.0` (Dart 3 compatible)
* Upgraded `socket_io_common` to `^3.0.0` (Socket.IO v4.7+ protocol, Dart 3)
  * All internal `src/` imports replaced with `package:socket_io_common/socket_io_common.dart`
  * `ERROR` packet-type constant renamed to `CONNECT_ERROR` (protocol v5 alignment)
  * `PacketParser.decodePayload` is now synchronous (returns `List`); callback pattern removed
  * `PacketParser.encodePayload` no longer accepts `supportsBinary` parameter
  * `PacketParser.decodePacket` second parameter is now positional `binaryType` (not `utf8decode`)
* Upgraded `uuid` to `^4.0.0`
* Replaced retired `pedantic` dev-dependency with `lints: ^3.0.0`
* Updated `analysis_options.yaml` to include `package:lints/recommended.yaml`

**Bug Fixes (Dart 3 soundness):**

* `namespace.dart`: Fixed JS-style boolean truthiness check `if (err)` → `if (err != null)`
* `namespace.dart`: Fixed JS-style `||` null-coalesce `err.data || err.message` → `err.data ?? err.message`
* `server.dart`: Fixed JS-style `if (err)` → `if (err != null)` in `set()` authorization callback
* `server.dart`: Fixed JS-style `&&` truthiness `oldSettings[key] && engine![...]` → null checks
* `engine/server.dart`: Fixed null-safety in `abortConnection` — `ServerErrorMessages[code]` is `String?`
* `polling_transport.dart`: Added null guard for `maxHttpBufferSize` comparison

## 1.0.1

**Bug Fix:**

* [#45](https://github.com/rikulo/socket.io-dart/issues/45) [BUG] Calling close on Server raises ConcurrentModificationError

**New Feature:**

* [#47](https://github.com/rikulo/socket.io-dart/pull/47) Added Future to start and stop server

## 1.0.0

**New Feature:**

* [#33](https://github.com/rikulo/socket.io-dart/issues/33) Null safety


## 0.9.4

**Bug Fix:**

* [#23](https://github.com/rikulo/socket.io-dart/issues/23) [BUG] Calling close on Server raises ConcurrentModificationError

## 0.9.3

**Bug Fix:**

* [#18](https://github.com/rikulo/socket.io-dart/pull/18) make sure it is accessing the rooms map


## 0.9.2

**Bug Fix:**

* [#17](https://github.com/rikulo/socket.io-dart/pull/17) inference error


## 0.9.1+1

**New Feature:**

* [#16](https://github.com/rikulo/socket.io-dart/pull/16) Apply Pedantic recommendations
