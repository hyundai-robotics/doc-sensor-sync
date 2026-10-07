# 1.6.6 Troubleshooting

If motion differs from what is expected, first stop the equipment safely according to site procedures. The table below lists **checks to perform after stopping**. It does not instruct you to change wiring, signal numbers, or resolution during operation, or to bypass safety interlocks.

## Checks by Symptom

| Symptom | Items to Check | Relevant Step |
| --- | --- | --- |
| Limit switch input remains at 0 or 1 | Selected sensor, input number, general I/O monitor, actual sensor state and wiring | [2. Checking Input Signals](2-check-inputs.md) |
| The conveyor moves but raw pulses do not change | Starting number of the 16-bit input, board settings and status, encoder connection and power | [1. Basic Settings](1-basic-settings.md), [2. Checking Input Signals](2-check-inputs.md) |
| Raw pulses wrap from ffff to 0 | Check whether this is normal 16-bit counter wraparound. Also check position value continuity and travel direction | [2. Checking Input Signals](2-check-inputs.md) |
| Actual travel distance does not match the workpiece position | Linear/circular conveyor selection, pulse/m units, use of the same reference point, slippage, tool/TCP | [3. Setting the Travel Direction and Resolution](3-calibration.md) |
| Multiple entries are registered for a single workpiece | Repeated limit switch signals, existing data, duplicate use of the manual limit switch button or cv.input | [2. Checking Input Signals](2-check-inputs.md), [5. Restart](5-first-run.md) |
| The program keeps waiting at cv.wait | Whether the actual workpiece has been detected, workpiece position and waiting distance, whether the conveyor is moving, whether a reset occurred after entry | [4. Program Structure](4-teach-and-program.md) |
| The robot enters the synchronized section without waiting | Existing workpiece data, whether the actual position already equals or exceeds the waiting distance, waiting distance units and command placement | [4. Program Structure](4-teach-and-program.md), [5. Restart](5-first-run.md) |
| The robot tracks in the opposite or an unexpected direction | Selected sensor, angle calculation program and S1/S2 order, counter settings and wiring, tool/TCP, sensor data in the steps | [3. Settings Verification](3-calibration.md), [4. Teaching](4-teach-and-program.md) |
| Tracking error is large or interference is expected during exit | Conveyor speed and robot tracking capability, travel direction and resolution, teaching reference, step data, and exit path | [3. Settings Verification](3-calibration.md), [4. Teaching](4-teach-and-program.md) |
| Playback after an interruption differs from the first test | Remaining workpieces, data cleared by reset, playback start step, robot's current position | [5. Restart](5-first-run.md) |

## When an Error Is Displayed

| Error | Checks and Guidance |
| --- | --- |
| E0019 Conveyor pulse frequency exceeds the allowable limit | Check the pulse input, board settings, and actual operating conditions. Do not attempt to resolve this by arbitrarily increasing the allowed pulse anomaly detection count. |
| E0021 Conveyor speed exceeds the allowable limit | Check the actual speed, resolution and units, input abnormalities, and allowable speed setting together. Do not continue testing by disabling system error detection. |

For details, see [3.3 Sensor Synchronization Parameters](../../3-user-interface/3-3-sensor-sync-parameter.md).
