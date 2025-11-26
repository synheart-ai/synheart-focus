# Synheart Focus API Reference

## Overview

This document provides API reference for all Synheart Focus SDKs. APIs are consistent across platforms with platform-idiomatic naming conventions.

## Core Concepts

### FocusState

Represents the current focus state at a point in time.

**Properties**:
- `focus_score` (float): Continuous focus estimate (0.0-1.0)
- `focus_label` (string): Discrete state (focused, distracted, scattered, fatigued)
- `focus_trend` (string): Trend direction (increasing, decreasing, stable)
- `cognitive_load` (string): Workload estimate (low, normal, high)
- `deep_focus_block` (bool): Sustained focus flag
- `fatigue_risk` (float): Focus decline likelihood (0.0-1.0)
- `timestamp` (int): Unix timestamp

### HSIData

Input data from HSI Runtime.

**Properties**:
- `hr` (float): Heart rate in BPM
- `hrv_rmssd` (float): HRV RMSSD value
- `stress_index` (float): Stress level (0.0-1.0)
- `motion_intensity` (float): Motion intensity (0.0-1.0)
- `hsi_embedding` (array): 64D HSI embedding vector

### BehaviorData

Input data from Behavior SDK.

**Properties**:
- `task_switch_rate` (float): App switching frequency (0.0-1.0)
- `interaction_burstiness` (float): Interaction variability (0.0-1.0)
- `idle_ratio` (float): Idle time proportion (0.0-1.0)
- `notification_interruptions` (int): Number of interruptions

## Python API

### FocusEngine

```python
from synheart_focus import FocusEngine, FocusConfig, FocusState

# Initialize
config = FocusConfig()
engine = FocusEngine.from_config(config)

# Subscribe to updates
def on_update(state: FocusState):
    print(f"Focus: {state.focus_score}")

engine.subscribe(on_update)

# Infer focus state
state = engine.infer(hsi_data, behavior_data)
```

### FocusConfig

```python
class FocusConfig:
    window_size: int = 60  # seconds
    update_interval: int = 120  # seconds
    model_path: Optional[str] = None
```

## Dart/Flutter API

```dart
import 'package:synheart_focus/synheart_focus.dart';

// Initialize
final engine = FocusEngine.initialize(
  config: FocusConfig(),
);

// Subscribe to updates
engine.onUpdate.listen((state) {
  print('Focus: ${state.focusScore}');
});

// Infer focus state
final state = await engine.infer(hsiData, behaviorData);
```

## Kotlin/Android API

```kotlin
import ai.synheart.focus.FocusEngine
import ai.synheart.focus.FocusState

// Initialize
val engine = FocusEngine.initialize(
  config = FocusConfig()
)

// Subscribe to updates
engine.onUpdate.collect { state ->
  println("Focus: ${state.focusScore}")
}

// Infer focus state
val state = engine.infer(hsiData, behaviorData)
```

## Swift/iOS API

```swift
import SynheartFocus

// Initialize
let engine = FocusEngine.initialize(
  config: FocusConfig()
)

// Subscribe to updates
engine.onUpdate.sink { state in
  print("Focus: \(state.focusScore)")
}

// Infer focus state
let state = engine.infer(hsiData: hsiData, behaviorData: behaviorData)
```

## Error Handling

All SDKs provide consistent error types:

- `FocusError.configuration`: Invalid configuration
- `FocusError.model`: Model loading/initialization error
- `FocusError.inference`: Inference computation error
- `FocusError.input`: Invalid input data

---

**For detailed platform-specific documentation, see individual SDK repositories.**

