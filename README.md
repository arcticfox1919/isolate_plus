
# isolate_plus

A modernized fork of the discontinued [dart-archive/isolate](https://github.com/dart-archive/isolate) package, updated to support Dart SDK 3.3.0 and above.

## About This Fork

The original `isolate` package was discontinued by the Dart team. This fork (`isolate_plus`) aims to:
- ✅ Support modern Dart SDK versions (3.3.0+)
- ✅ Maintain compatibility with the latest Dart language features
- ✅ Fix compatibility issues with newer Dart SDK APIs (e.g., `RawReceivePort.keepIsolateAlive`)
- ✅ Provide continued maintenance and updates

## Overview

Helps with isolates and isolate communication in Dart.
Requires the `dart:isolate` library being available.
Isolates are not available for Dart on the web.

The package contains individual libraries with different purposes.

### Creating send ports and responding to messages

The "ports.dart" sub-library contains functionality
for creating `SendPort`s and reacting to values sent to those ports.

Key utilities include:
- **`singleCallbackPort`** / **`singleCallbackPortWithTimeout`** - Create a port that calls a callback once with the first message
- **`singleCompletePort`** - Create a port that completes a `Completer` with the first message
- **`singleResponseFuture`** / **`singleResponseFutureWithTimeout`** - Create a `Future` completed by a single port message
- **`singleResultFuture`** - Handle future results sent across isolates
- **`SingleResponseChannel`** - A reusable pattern for single-message request-response communication
- **`sendFutureResult`** - Send a future's result (value or error) across isolates

### Working with isolates and running functions in other isolates.

The "isolate_runner.dart" sub-library introduces an `IsolateRunner` class
that gives easy access to the `Isolate` functionality, and also
gives a way to run new functions in the isolate repeatedly, instead of
just on the initial `spawn` call.

### A central registry for values that can be used across isolates.

The "registry.dart" sub-library provides a way to create an
object registry, and give access to it across different isolates.

### Balancing load across several isolates.

The "load_balancer.dart" sub-library can manage multiple `Runner` objects,
including `IsolateRunner`, and run functions on the currently least loaded
runner.

## Migration from `isolate` package

If you're migrating from the original `isolate` package, simply update your `pubspec.yaml`:

```yaml
dependencies:
  isolate_plus: ^2.2.0  # Instead of isolate: ^2.1.1
```

And update your imports:

```dart
// Old
import 'package:isolate/isolate.dart';

// New
import 'package:isolate_plus/isolate_plus.dart';
```

The API remains fully compatible with the original package.

## Features and bugs

Please file feature requests and bugs at the [issue tracker][tracker].

[tracker]: https://github.com/arcticfox1919/isolate_plus/issues

