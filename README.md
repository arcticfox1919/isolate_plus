
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

## Quick Start

### Using IsolateRunner

Run functions in a separate isolate with easy-to-use API:

```dart
import 'package:isolate_plus/isolate_runner.dart';

// Computation function (can be top-level or static)
int expensiveWork(int value) {
  // Simulate heavy computation
  return value * value;
}

void main() async {
  // Spawn an isolate
  final runner = await IsolateRunner.spawn();
  
  // Run functions in the isolate
  final result = await runner.run(expensiveWork, 42);
  print('Result: $result'); // Result: 1764
  
  // Close when done
  await runner.close();
}
```

### Using LoadBalancer

Distribute work across multiple isolates automatically:

```dart
import 'package:isolate_plus/load_balancer.dart';

int heavyTask(int n) {
  // Some CPU-intensive work
  return List.generate(n, (i) => i * i).reduce((a, b) => a + b);
}

void main() async {
  // Create a load balancer with 4 isolates
  final balancer = await LoadBalancer.create(4, IsolateRunner.spawn);
  
  // Run tasks in parallel across isolates
  final results = await Future.wait([
    balancer.run(heavyTask, 1000),
    balancer.run(heavyTask, 2000),
    balancer.run(heavyTask, 3000),
  ]);
  
  print('Results: $results');
  
  // Close when done
  await balancer.close();
}
```

## Features

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

### Working with isolates and running functions in other isolates

The "isolate_runner.dart" sub-library introduces an `IsolateRunner` class
that gives easy access to the `Isolate` functionality, and also
gives a way to run new functions in the isolate repeatedly, instead of
just on the initial `spawn` call.

### A central registry for values that can be used across isolates

The "registry.dart" sub-library provides a way to create an
object registry, and give access to it across different isolates.

### Balancing load across several isolates

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

## Issues and Contributions

Please file feature requests and bugs at the [issue tracker][tracker].

[tracker]: https://github.com/arcticfox1919/isolate_plus/issues
