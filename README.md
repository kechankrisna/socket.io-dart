# socket_io_plus

A Dart 3 compatible port of the [Socket.IO](https://socket.io) server library.  
Real-time, bidirectional, event-based communication — fully null-safe and ready for modern Dart.

[![pub package](https://img.shields.io/pub/v/socket_io_plus.svg)](https://pub.dev/packages/socket_io_plus)
[![Dart SDK](https://img.shields.io/badge/Dart-%3E%3D3.0.0-blue)](https://dart.dev)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

---

## Features

- Full Dart 3 null-safety support
- WebSocket and XHR/JSONP polling transports
- Namespace (multiplexing) support
- Room-based broadcasting
- In-memory socket adapter
- Compatible with the official [socket_io_client](https://pub.dev/packages/socket_io_client) and JS Socket.IO clients

---

## Installation

Add to your `pubspec.yaml`:

```yaml
dependencies:
  socket_io_plus: ^2.0.0
```

Then run:

```sh
dart pub get
```

---

## Quick Start

### Server (Dart)

```dart
import 'package:socket_io_plus/socket_io.dart';

void main() async {
  final io = Server();

  // Default namespace
  io.on('connection', (Socket client) {
    print('Client connected: ${client.id}');

    client.on('msg', (data) {
      print('Message from client: $data');
      client.emit('fromServer', 'ok');
    });

    client.on('disconnect', (_) {
      print('Client disconnected: ${client.id}');
    });
  });

  // Custom namespace
  final nsp = io.of('/chat');
  nsp.on('connection', (Socket client) {
    print('Client connected to /chat');

    client.on('message', (data) {
      // Broadcast to everyone in the namespace
      nsp.emit('broadcastMessage', data);
    });
  });

  await io.listen(3000);
  print('Server running on port 3000');
}
```

### Client — JavaScript

```js
const socket = io('http://localhost:3000');

socket.on('connect', () => console.log('connected'));
socket.on('fromServer', (data) => console.log('from server:', data));
socket.on('disconnect', () => console.log('disconnected'));

socket.emit('msg', 'hello');
```

### Client — Dart

```dart
import 'package:socket_io_client/socket_io_client.dart' as IO;

void main() {
  final socket = IO.io('http://localhost:3000');

  socket.on('connect', (_) {
    print('connected');
    socket.emit('msg', 'hello from Dart');
  });

  socket.on('fromServer', (data) => print('from server: $data'));
  socket.on('disconnect', (_) => print('disconnected'));
}
```

---

## Rooms

Sockets can join and leave named rooms. Broadcast to all members of a room:

```dart
io.on('connection', (Socket client) {
  client.join('room1');

  client.on('msg', (data) {
    // Emit to all sockets in 'room1'
    io.to('room1').emit('roomMessage', data);
  });
});
```

---

## Namespaces

Create separate communication channels that share the same underlying connection:

```dart
final admin = io.of('/admin');
admin.on('connection', (Socket client) {
  client.emit('welcome', 'You are in the admin namespace');
});
```

---

## Transports

Powered by Engine.IO, two transports are supported:

| Transport   | Description                                    |
|-------------|------------------------------------------------|
| `polling`   | XHR / JSONP long-polling (HTTP fallback)       |
| `websocket` | Full-duplex WebSocket connection               |

Clients start with polling and upgrade to WebSocket automatically when available.

---

## Migration from `socket_io`

This package is a Dart 3 compatible fork of [`socket_io`](https://pub.dev/packages/socket_io). To migrate:

1. Replace `socket_io` with `socket_io_plus` in `pubspec.yaml`
2. Update all imports:

```dart
// Before
import 'package:socket_io/socket_io.dart';

// After
import 'package:socket_io_plus/socket_io.dart';
```

No other code changes are required.

---

## Dart Client Package

Use [`socket_io_client`](https://pub.dev/packages/socket_io_client) to connect a Dart client to this server.

---

## Contributing

Contributions are welcome! Please open an issue or submit a pull request.

If you are new to Git or GitHub, read [this guide](https://help.github.com/) first.

---

## License

This project is licensed under the [MIT License](LICENSE).