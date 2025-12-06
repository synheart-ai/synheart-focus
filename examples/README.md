# Synheart Focus Examples

This directory contains example applications demonstrating how to use Synheart Focus SDKs.

## Flutter Example

A complete Flutter application that demonstrates:
- Loading and using the CNN-LSTM ONNX model
- Computing focus scores and focus states
- Real-time display of focus inference results

See [flutter_example/README.md](./flutter_example/README.md) for setup and usage instructions.

## Running the Flutter Example

```bash
cd flutter_example
flutter pub get
flutter run
```

The app will:
1. Load the ONNX model from assets
2. Generate dummy heart rate and RR interval data
3. Compute focus scores in real-time
4. Display results with a beautiful UI


