# Flutter Example App - Synheart Focus

A simple Flutter example app to test Synheart Focus and Focus Score on a Flutter emulator.

## Features

- Loads the CNN-LSTM ONNX model for focus detection
- Generates dummy heart rate and RR interval data
- Computes focus score and focus state in real-time
- Displays results with a beautiful UI

## Setup

1. Make sure you have Flutter installed and configured
2. Navigate to this directory:
   ```bash
   cd synheart-focus/examples/flutter_example
   ```

3. Get dependencies:
   ```bash
   flutter pub get
   ```

4. Make sure the models are in place:
   - `assets/models/cnn_lstm_top_6_features.onnx`
   - `assets/models/scaler_info_top_6_features.json`

## Running the App

1. Start a Flutter emulator or connect a device:
   ```bash
   flutter emulators --launch <emulator_name>
   # OR
   flutter devices
   ```

2. Run the app:
   ```bash
   flutter run
   ```

## Usage

1. Wait for the model to initialize (status will show "Model loaded successfully")
2. Click "Start Test" to begin generating dummy data and computing focus scores
3. Watch the focus score and state update in real-time
4. View the results history to see past inferences
5. Click "Stop Test" to stop the test

## How It Works

The app:
- Generates realistic dummy heart rate (HR) and RR interval data
- Uses a sliding window buffer (60 seconds, 5 second step)
- Extracts HRV features (MEDIAN_RR, HR, MEAN_RR, SDRR_RMSSD, pNN25, higuci)
- Runs inference using the ONNX model
- Computes focus score (0-100) and focus state (Focused, time pressure, Distracted)
- Displays results with visual indicators

## Model Information

The app uses the CNN-LSTM model trained on top 6 HRV features:
- MEDIAN_RR: Median RR interval
- HR: Heart rate (mean)
- MEAN_RR: Mean RR interval
- SDRR_RMSSD: Standard deviation of RR intervals
- pNN25: Percentage of NN intervals differing by >25ms
- higuci: Higuchi fractal dimension

## Troubleshooting

- **Model not loading**: Check that the model files are in `assets/models/` and listed in `pubspec.yaml`
- **No results**: Make sure enough data points are generated (at least 10-15 data points for a window)
- **Import errors**: Run `flutter pub get` again


