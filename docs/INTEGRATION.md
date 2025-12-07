# Synheart Focus Integration Guide

This guide explains how to integrate Synheart Focus into your applications across different platforms and use cases.

## Table of Contents

1. [Overview](#overview)
2. [Integration Patterns](#integration-patterns)
3. [Platform-Specific Integration](#platform-specific-integration)
4. [Data Flow](#data-flow)
5. [Best Practices](#best-practices)
6. [Troubleshooting](#troubleshooting)

## Overview

Synheart Focus is designed to integrate seamlessly with:

- **Synheart Wear SDK**: Provides HSI (biosignal) data
- **Synheart Behavior SDK**: Provides behavioral interaction patterns
- **Syni Core**: Consumes focus states for adaptive behavior
- **Syni Life**: Displays focus insights and trends
- **SWIP**: Labels digital sessions by focus level
- **Custom Applications**: Any app needing focus awareness

## Integration Patterns

### Pattern 1: Real-Time Focus Monitoring

**Use Case**: Display live focus score in UI, adjust app behavior based on focus state

```python
# Python
from synheart_focus import FocusEngine, FocusConfig

engine = FocusEngine.from_config(FocusConfig())

def on_focus_update(focus_state):
    # Update UI
    update_focus_widget(focus_state.focus_score)

    # Adaptive behavior
    if focus_state.focus_label == 'distracted':
        suggest_break()

engine.subscribe(on_focus_update)
```

```dart
// Flutter/Dart
final focusEngine = FocusEngine.initialize(config: FocusConfig());

focusEngine.onUpdate.listen((focusState) {
  // Update UI
  setState(() => _focusScore = focusState.focusScore);

  // Adaptive behavior
  if (focusState.focusLabel == 'Low Focus') {
    showBreakSuggestion();
  }
});
```

### Pattern 2: Batch Analysis

**Use Case**: Analyze focus patterns over time, generate insights

```python
# Collect focus states over time
focus_history = []

def collect_focus_state(state):
    focus_history.append({
        'timestamp': state.timestamp,
        'score': state.focus_score,
        'label': state.focus_label
    })

engine.subscribe(collect_focus_state)

# Later: analyze patterns
avg_focus = sum(s['score'] for s in focus_history) / len(focus_history)
peak_hours = identify_peak_focus_hours(focus_history)
```

### Pattern 3: Event-Driven Triggers

**Use Case**: Trigger actions based on focus state changes

```kotlin
// Kotlin
engine.onUpdate.collect { state ->
    when {
        state.focusLabel == FocusLabel.PEAK -> {
            // Notify user of peak performance
            notifyPeakFocus()
        }
        state.focusScore < 0.3 -> {
            // Suggest break
            suggestBreak()
        }
        state.focusLabel == FocusLabel.DISTRACTED -> {
            // Block distracting notifications
            enableDoNotDisturb()
        }
    }
}
```

## Platform-Specific Integration

### Python Integration

#### With HSI Runtime

```python
from synheart_focus import FocusEngine, FocusConfig
from synheart_hsi import HSIRuntime

# Initialize HSI Runtime
hsi_runtime = HSIRuntime()

# Initialize Focus Engine
focus_engine = FocusEngine.from_config(FocusConfig())

# Connect HSI to Focus
hsi_runtime.subscribe(lambda hsi_state: {
    # Get behavior data from Behavior SDK
    behavior_data = get_behavior_data()

    # Infer focus
    focus_state = focus_engine.infer(hsi_state, behavior_data)
})
```

#### With Synheart Behavior

```python
from synheart_behavior import BehaviorTracker

# Initialize behavior tracker
behavior_tracker = BehaviorTracker()

# Track digital behavior
behavior_tracker.start()

# Get current behavior data
behavior_data = behavior_tracker.get_current_state()

# Combine with HSI for focus inference
focus_state = focus_engine.infer(hsi_data, behavior_data)
```

### Flutter/Dart Integration

#### In a Flutter App

```dart
import 'package:synheart_focus/synheart_focus.dart';

class FocusWidget extends StatefulWidget {
  @override
  _FocusWidgetState createState() => _FocusWidgetState();
}

class _FocusWidgetState extends State<FocusWidget> {
  late FocusEngine _focusEngine;
  FocusState? _currentState;

  @override
  void initState() {
    super.initState();

    // Initialize engine
    _focusEngine = FocusEngine.initialize(
      config: FocusConfig(),
    );

    // Subscribe to updates
    _focusEngine.onUpdate.listen((state) {
      setState(() => _currentState = state);
    });
  }

  @override
  Widget build(BuildContext context) {
    if (_currentState == null) {
      return CircularProgressIndicator();
    }

    return Column(
      children: [
        Text('Focus: ${_currentState!.focusLabel}'),
        LinearProgressIndicator(
          value: _currentState!.focusScore,
        ),
      ],
    );
  }

  @override
  void dispose() {
    _focusEngine.dispose();
    super.dispose();
  }
}
```

#### With Background Processing

```dart
import 'package:workmanager/workmanager.dart';

void callbackDispatcher() {
  Workmanager().executeTask((task, inputData) async {
    // Collect HSI and behavior data
    final hsiData = await collectHSIData();
    final behaviorData = await collectBehaviorData();

    // Infer focus
    final focusEngine = FocusEngine.initialize();
    final state = await focusEngine.infer(hsiData, behaviorData);

    // Store for later analysis
    await storeFocusState(state);

    return Future.value(true);
  });
}
```

### Android/Kotlin Integration

#### In an Android Activity

```kotlin
import ai.synheart.focus.*
import androidx.lifecycle.lifecycleScope
import kotlinx.coroutines.launch

class MainActivity : AppCompatActivity() {
    private lateinit var focusEngine: FocusEngine

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)

        // Initialize engine
        focusEngine = FocusEngine.initialize(
            config = FocusConfig()
        )

        // Observe focus updates
        lifecycleScope.launch {
            focusEngine.onUpdate.collect { state ->
                state?.let { updateUI(it) }
            }
        }
    }

    private fun updateUI(state: FocusState) {
        focusScoreText.text = "Focus: ${state.focusScore}"
        focusLabelText.text = state.focusLabel.name
    }
}
```

#### With WorkManager for Background Tasks

```kotlin
import androidx.work.CoroutineWorker
import androidx.work.WorkerParameters

class FocusAnalysisWorker(
    context: Context,
    params: WorkerParameters
) : CoroutineWorker(context, params) {

    override suspend fun doWork(): Result {
        val focusEngine = FocusEngine.initialize()

        // Collect data
        val hsiData = collectHSIData()
        val behaviorData = collectBehaviorData()

        // Infer focus
        val state = focusEngine.infer(hsiData, behaviorData)

        // Store or upload
        storeFocusState(state)

        return Result.success()
    }
}
```

### iOS/Swift Integration

#### In a SwiftUI View

```swift
import SwiftUI
import SynheartFocus
import Combine

struct FocusView: View {
    @StateObject private var viewModel = FocusViewModel()

    var body: some View {
        VStack {
            Text("Focus: \(viewModel.focusLabel)")
                .font(.headline)

            ProgressView(value: viewModel.focusScore)
                .progressViewStyle(.linear)

            Text("\(Int(viewModel.focusScore * 100))%")
                .font(.caption)
        }
        .padding()
        .onAppear { viewModel.startMonitoring() }
        .onDisappear { viewModel.stopMonitoring() }
    }
}

class FocusViewModel: ObservableObject {
    @Published var focusScore: Double = 0.0
    @Published var focusLabel: String = "Unknown"

    private var focusEngine: FocusEngine?
    private var cancellables = Set<AnyCancellable>()

    func startMonitoring() {
        focusEngine = FocusEngine.initialize(config: FocusConfig())

        focusEngine?.onUpdate
            .receive(on: DispatchQueue.main)
            .sink { [weak self] state in
                self?.focusScore = state.focusScore
                self?.focusLabel = state.focusLabel
            }
            .store(in: &cancellables)
    }

    func stopMonitoring() {
        cancellables.removeAll()
        focusEngine = nil
    }
}
```

## Data Flow

### Complete Integration Flow

```
┌─────────────────┐
│  Wearable       │
│  (HR, HRV)      │
└────────┬────────┘
         │
         ▼
┌─────────────────┐      ┌──────────────────┐
│  HSI Runtime    │◄─────┤  Phone Sensors   │
│  (Fusion)       │      │  (Motion, GPS)   │
└────────┬────────┘      └──────────────────┘
         │
         ▼
      HSIData
         │
         ├──────────────┐
         │              │
         ▼              ▼
┌─────────────────┐  ┌──────────────────┐
│  Focus Engine   │  │  Behavior SDK    │
│                 │◄─┤  (Interactions)  │
└────────┬────────┘  └──────────────────┘
         │               BehaviorData
         ▼
    FocusState
         │
         ├────────┬─────────┬──────────┐
         │        │         │          │
         ▼        ▼         ▼          ▼
     ┌─────┐ ┌──────┐ ┌──────┐  ┌──────────┐
     │ UI  │ │ Syni │ │ SWIP │  │  Cloud   │
     └─────┘ └──────┘ └──────┘  └──────────┘
```

### Data Requirements

#### Minimum HSI Data
- `hr`: Heart rate in BPM (required)
- `hrv_rmssd`: HRV RMSSD in ms (required)
- `stress_index`: 0.0-1.0 (required)
- `motion_intensity`: 0.0-1.0 (required)

#### Minimum Behavior Data
- `task_switch_rate`: Switches per minute (required)
- `interaction_burstiness`: 0.0-1.0 (required)
- `idle_ratio`: 0.0-1.0 (required)

#### Optional Context Data
- `notification_interruptions`: Count
- `hsi_embedding`: 64D vector
- `circadian_phase`: Time of day context
- `sleep_deficit`: Recent sleep quality
- `recovery_score`: Physiological recovery

## Best Practices

### 1. Error Handling

```python
# Python
try:
    focus_state = engine.infer(hsi_data, behavior_data)
except FocusError.input as e:
    # Handle invalid input data
    logger.error(f"Invalid input: {e}")
except FocusError.inference as e:
    # Handle inference failure
    logger.error(f"Inference failed: {e}")
```

```dart
// Dart
try {
  final state = await focusEngine.infer(hsiData, behaviorData);
} on FocusException catch (e) {
  print('Focus inference error: ${e.message}');
}
```

### 2. Data Quality Checks

```python
def validate_hsi_data(hsi_data):
    """Ensure HSI data is within reasonable ranges"""
    assert 40 <= hsi_data.hr <= 200, "HR out of range"
    assert 5 <= hsi_data.hrv_rmssd <= 200, "HRV out of range"
    assert 0.0 <= hsi_data.stress_index <= 1.0, "Stress index out of range"
    assert 0.0 <= hsi_data.motion_intensity <= 1.0, "Motion out of range"
```

### 3. Update Frequency

- **Real-time apps**: Update every 60-120 seconds
- **Batch analysis**: Update every 5-15 minutes
- **Background tasks**: Update every 30-60 minutes

### 4. Resource Management

```dart
// Always dispose engines when done
@override
void dispose() {
  focusEngine.dispose();
  super.dispose();
}
```

### 5. Privacy Considerations

- Always obtain user consent before collecting behavioral data
- Store focus states locally by default
- Only upload to cloud if explicitly enabled
- Anonymize data before any aggregation
- Provide clear data deletion mechanisms

## Troubleshooting

### Common Issues

#### Issue: Focus score always returns 0.5

**Cause**: Likely missing or invalid input data

**Solution**:
```python
# Check data quality
print(f"HSI: {hsi_data}")
print(f"Behavior: {behavior_data}")

# Validate ranges
assert all([
    40 <= hsi_data.hr <= 200,
    5 <= hsi_data.hrv_rmssd <= 200,
    0.0 <= hsi_data.stress_index <= 1.0,
])
```

#### Issue: Subscription not receiving updates

**Cause**: Engine not initialized or data not flowing

**Solution**:
```python
# Verify subscription
engine.subscribe(lambda state: print(f"Received: {state}"))

# Check data flow
hsi_data = get_hsi_data()
behavior_data = get_behavior_data()
state = engine.infer(hsi_data, behavior_data)
```

#### Issue: High memory usage

**Cause**: Storing too much history or not disposing engines

**Solution**:
```dart
// Limit history size
final maxHistorySize = 1000;
if (focusHistory.length > maxHistorySize) {
  focusHistory.removeAt(0);
}

// Always dispose
focusEngine.dispose();
```

#### Issue: Inconsistent results

**Cause**: Input data quality issues or temporal smoothing

**Solution**:
- Enable debug logging to inspect raw inputs
- Check data collection intervals
- Adjust smoothing factors in config
- Verify sensor calibration

### Debug Mode

```python
# Python
config = FocusConfig(
    enable_debug_logging=True
)
engine = FocusEngine.from_config(config)
```

```dart
// Dart
final config = FocusConfig(
  enableDebugLogging: true,
);
```

### Getting Help

- **Documentation**: [docs.synheart.ai](https://docs.synheart.ai)
- **GitHub Issues**: [synheart-focus/issues](https://github.com/synheart-ai/synheart-focus/issues)
- **API Reference**: [API_REFERENCE.md](API_REFERENCE.md)
- **Architecture Guide**: [ARCHITECTURE.md](ARCHITECTURE.md)

---

**Last Updated**: 2025-12-06
**Author**: Synheart Research & Engineering
