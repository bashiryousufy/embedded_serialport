<!--
This README describes the package. If you publish this package to pub.dev,
this README's contents appear on the landing page for your package.

For information about how to write a good package README, see the guide for
[writing package pages](https://dart.dev/guides/libraries/writing-package-pages).

For general information about developing packages, see the Dart guide for
[creating packages](https://dart.dev/guides/libraries/create-library-packages)
and the Flutter guide for
[developing packages and plugins](https://flutter.dev/developing-packages).
-->

# embedded_serialport

`embedded_serialport` is a Flutter package for serial port communication, it uses Dart FFI (Foreign Function Interface) to interact with Rust and C shared libraries. This package provides a simple API to list available serial ports and perform read/write operations.

## Platform Support
|   Linux:  | Ubuntu |

## Features

- List available serial ports
- Open and configure serial ports
- Read from and write to serial ports
- Set timeouts for serial communication

## Installation

To install `embedded_serialport`, use either `flutter pub add` or `dart pub add`, depending on your project type:

### Flutter

```bash
flutter pub add embedded_serialport
```
### Dart

```bash
dart pub add embedded_serialport
```

## Usage
Import the package in your Dart code:
```dart
import 'package:embedded_serialport/embedded_serialport.dart';
```

## Example 1: Listing Available Serial Ports
```dart
import 'package:embedded_serialport/embedded_serialport.dart';

void main() {
  List<String> ports = Serial.getPorts();
  // print(ports.where((x) => x.endsWith("USB0")));
  print(ports);
  for (int i = 0; i < ports.length; i++) {
    print(ports[i]);
  }
}

```
## Example 2: Communicating with a Serial Device
```dart
import 'package:embedded_serialport/embedded_serialport.dart';

void main() {
  List<String> ports = Serial.getPorts();
  // print(ports);

  // '/dev/ttyUSB0'
  var s = Serial(ports.first, Baudrate.b115200);
  s.timeout(2);

  try {
    s.writeString('led,0');
    var event = s.read(20);
    print(event.toString());
    s.writeString('led');
    var eventt = s.read(20);
    print(eventt.toString());
  } finally {
    s.dispose();
  }
}

```
## API
`Serial`
### Static Methods
- `List<String> getPorts()`
  - Returns a list of available serial ports.
### Constructor
- `Serial(String port, Baudrate baudrate)`
  - Creates a new instance of Serial for the specified port and baud rate.
### Methods
- `void timeout(int seconds)`
  - Sets a timeout for read/write operations.
- `void writeString(String data)`
  - Writes a string to the serial port.
- `Uint8List read(int length)`
  - `Reads a specified number of bytes from the serial port.`
- `void dispose()`
  - Closes the serial port and releases resources.

### `Baudrate`
Enumeration of common baud rates:

- `Baudrate.b9600`
- `Baudrate.b19200`
- `Baudrate.b38400`
- `Baudrate.b57600`
- `Baudrate.b115200`
- And more...