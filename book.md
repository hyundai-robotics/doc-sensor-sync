
[__SOURCE](README.md)
# ${cont_model} Controller Function Manual - Sensor Synchronization (Conveyor, Press)

Sensor synchronization is a feature that links the robot to a moving conveyor workpiece or press by receiving external sensor signals, such as from an encoder.

## Where should I begin?
**If you are setting up sensor synchronization for the first time**: Follow the steps in sequence—from preparation to input checks, settings, teaching, and initial test runs—in the [Tutorial](1-intro/1-6-getting-started/README.md). This course is intended for users who have completed basic robot operation and safety training.

**If you want to understand the principles and system configuration**: Refer to [1. Overview](1-intro/README.md) and [2. System Configuration & Connection](2-system-config-access/README.md).

**If you are looking for screens, parameters, or commands**: Refer to [3. User Interface](3-user-interface/README.md).

**If you are using press synchronization**: Check [Press Synchronization Principles](1-intro/1-3-press-sync-principle.md) and [Teaching Press Synchronization](4-teaching/4-2-press-sync-teaching.md). Do not directly apply the conveyor tracking procedures.

**If you encounter issues during initial setup or testing**: Refer to the [Troubleshooting](1-intro/1-6-getting-started/6-troubleshooting.md) Checklist.

Before starting work, please read the [Pre-operation Precautions](0-about-this-manual/precautions.md) and [Safety Notices](0-about-this-manual/safety-notice.md). The values shown on screens and in examples are not recommended settings for all equipment; you must verify them according to your actual controller version and site specifications.

[__SOURCE](0-about-this-manual/README.md)
# About the Manual

[__SOURCE](0-about-this-manual/precautions.md)
# Precautions

{% include file="en/precautions.md" %}

[__SOURCE](0-about-this-manual/safety-notice.md)
# Safety Cautions

{% include file="en/safety-notice.md" %}

[__SOURCE](1-intro/README.md)
# 1. Overview

Sensor synchronization is a function that performs synchronization based on external sensor signals. External sensors support encoders and are categorized for conveyor and press operations.

- Conveyor synchronization

    The robot tracks the conveyor and operates on workpieces moving along it.

- Press synchronization

    The operation is performed by synchronizing the press's travel distance with the robot's position.

[__SOURCE](1-intro/1-1-system-config.md)
# 1.1 System Configuration

A typical configuration of the conveyor synchronization system is shown below.

![](../_assets/image9.png)

- **Limit switch**

    A device that notifies the controller whether a workpiece has entered a specific position on the conveyor and whether the press has passed a specific position. The position of the limit switch serves as the reference point for position judgment.

- **Encoder**

    An encoder that generates pulses corresponding to motor rotation is attached to the motor drive. The encoder connects to the robot controller, and the pulses output from the encoder are input to the robot controller.

[__SOURCE](1-intro/1-2-conveyor-sync-principle.md)
# 1.2 Conveyor Synchronization Principle

- **Teaching**

    For example, consider teaching points P1~P7 while the conveyor is stopped as shown below.

![](../_assets/image10-1.png)

- **Playback**

    If P2~P6 are set as the conveyor synchronization section and the taught trajectory is replayed, the robot must synchronize with the varying conveyor speed and maintain the relative position and orientation between the workpiece and the tool.
    Within the sync section, the workpiece shifts from the taught reference position by the distance the workpiece moved after passing the limit switch, as shown below.

![](../_assets/image10-2.png)

[__SOURCE](1-intro/1-3-press-sync-principle.md)
# 1.3 Press Synchronization Principle

The press moves from the top dead center down to the bottom dead center to perform the press operation, then rises back to the top, forming one cycle. Press synchronization records the press position and robot positions in step data to synchronize the robot position according to the press movement speed. Synchronization performance is limited by the robot's acceleration/deceleration and maximum speed; large variations from the expected press speed may cause errors.

![](../_assets/image11.png)

[__SOURCE](1-intro/1-4-major-spec.md)
# 1.4 Major Specifications

| **Item** | **Specification** |
| :------: | :---------------: |
| Number of synchronizable sensors (conveyor, press) | 2 |
| Conveyor (press) form | Linear, circular |
| Conveyor angle setting | Supports automatic setting |
| Pulse input type | Open collector, line drive |
| Pulse counting method | Up/Down |
| Encoder resolution setting | Supports automatic setting |
| Max number of workpieces allowed per conveyor | 100 |
| Synchronizable sensor travel distance | 21 m |
| Interpolation method in conveyor sync section | Linear (L), Circular (C) |
| Interpolation method in press sync section | Axis interpolation (P), Linear (L), Circular (C) |

[__SOURCE](1-intro/1-5-operation-sequence.md)
# 1.5 Operation Sequence

For initial setup, follow the steps below. Proceed to the next step only after verifying normal results at each stage. Detailed procedures using a straight conveyor and a single workpiece can be found in **[1.6 Tutorial](1-6-getting-started/README.md)**.

| Step | Task | Expected Result to Verify |
| --- | --- | --- |
| 1 | Verify safety procedures, system configuration, and I/O assignments; back up existing settings | Environment prepared for testing |
| 2 | Configure sensor to use, sync type/mode, and input/output signals | Settings match actual equipment |
| 3 | Verify limit switch and encoder inputs | Physical limit switch input changes and raw pulse variations |
| 4 | Set conveyor angle and encoder resolution | Match between actual movement direction/distance and settings |
| 5 | Teach sync section and verify step data | Verify robot position and sensor position data to be used |
| 6 | Add wait, sync start, and sync end commands | Verify syntax and command placement for a single-workpiece program |
| 7 | Perform initial test run and verify restart procedures | Confirm tracking, sync termination, and safe retract |

Below is the existing operational flowchart. Refer to the table above and the tutorial procedures together for actual operations and completion conditions.

![Sensor Sync Operational Flowchart](../_assets/image12.png)

For press synchronization, please refer separately to [4.2 Teaching Press Synchronization](../4-teaching/4-2-press-sync-teaching.md).
[__SOURCE](1-intro/1-6-getting-started/README.md)
# 1.6 Tutorial

This tutorial guides users who are setting up sensor synchronization for the first time through **checking input signals → setting the travel direction and resolution → teaching → performing the first test run**.

## Objective

Using one linear conveyor and one workpiece, verify that the robot tracks the workpiece over a short section and then moves away safely.
This tutorial is intended for **users who have completed basic robot operation and safety training**. If you are new to robot operation, first review the controller operation manual and complete the required safety training.

{% hint style="warning" %}
This tutorial does not replace the equipment's safety procedures. Read the [Safety Precautions](../../0-about-this-manual/safety-notice.md) first, and comply with the site's risk assessment, interlocks, and stopping procedures. Wiring work must be performed by authorized personnel with the power disconnected. Do not touch moving workpieces or operate the robot and conveyor simultaneously from inside the workspace.
{% endhint %}

## Before You Start

- Back up any existing sensor synchronization settings and job programs. Prepare a test program without overwriting an existing production program.
- Have the personnel responsible verify the installation and connections of the supported I/F board, encoder, and limit switch. Refer to [2. System Configuration and Connections](../../2-system-config-access/README.md).
- Prepare a test environment where the conveyor can be moved and stopped safely, and where potential interference between the robot, tool, and workpiece can be checked.
- Verify the tool/TCP and robot coordinate system to be used. Use the same reference in the angle calculation program and the test program.
- Agree with the personnel responsible for the equipment on the initial test speed, acceptance criteria for tracking error, and a safe test area. (The speeds shown in this manual's screens and examples are not recommended values.)

## Procedure Overview

| Step | Task | Requirements for Proceeding |
| --- | --- | --- |
| [1.6.1 Basic Settings](1-basic-settings.md) | Select the sensor and conveyor type, and assign signals | The settings match the actual equipment |
| [1.6.2 Checking Input Signals](2-check-inputs.md) | Check the limit switch and raw pulses | Limit switch input and pulse changes have been verified |
| [1.6.3 Setting the Travel Direction and Resolution](3-calibration.md) | Calculate the angle and resolution, and compare against actual travel | The travel direction and distance have been verified |
| [1.6.4 Teaching and Program Structure](4-teach-and-program.md) | Record the waiting, synchronized, and exit positions, and add commands | The step data and program structure have been checked |
| [1.6.5 First Test Run and Restart](5-first-run.md) | Test with a single workpiece, and check completion and restart | Tracking and a safe exit have been verified |
| [1.6.6 Troubleshooting](6-troubleshooting.md) | Check according to the symptoms | The cause has been resolved and checks have been repeated from the relevant step |

## Key Terms

- **Limit switch:** A sensor that detects workpiece entry. It serves as the reference point for determining the workpiece position.
- **Raw pulses:** The raw pulse count received from the encoder. The monitor displays it in hexadecimal, cycling through the range `0～ffff`.
- **Workpiece position:** The position to which the workpiece has traveled relative to the limit switch. For a linear conveyor, the unit is mm.
- **Encoder resolution:** The number of pulses generated when a linear conveyor travels 1 m.
- **Synchronized section:** A section executed with the robot position adjusted to follow a moving workpiece.

[__SOURCE](1-intro/1-6-getting-started/1-basic-settings.md)
# 1.6.1. Tutorial - Sensor Selection and Basic Settings

## Objective and Prerequisites

Select the sensor and linear conveyor to use in this tutorial, and configure the I/O assignments for the actual equipment. Complete [Tutorial - Before You Start](README.md) first.

## Procedure

1. Open `[Settings] > [Application Parameters] > [Sensor Synchronization]`.
2. Press the `[+]` button at the top right of the screen to add a sensor.
3. Select `Conveyor` for **Synchronization Status** and `Linear` for **Conveyor Type**.
4. Configure **Limit Switch Input**, **Pulse Counter Input (16 bit)**, and any other signals to be used. For the pulse counter, enter the starting signal number among the 16 inputs (the number of the least significant bit). Verify that these 16 consecutive signals are not also assigned to other purposes.
5. Check the **Pulse Counter Type** and **Pulse Communication Method** against the specifications of the actual encoder and I/F board. If you do not know the wiring or board specifications, consult the personnel responsible rather than selecting values arbitrarily.
6. Confirm operating parameters, such as the allowable speed, with the personnel responsible. The encoder resolution will be calculated and verified in [1.6.3 Setting the Angle and Encoder Resolution](3-calibration.md).
7. Use only one workpiece in this tutorial. Regardless of the maximum allowable workpiece count setting, prepare the test conditions so that no additional workpieces can enter.
8. Apply and save the settings, then reopen the screen to verify that the settings have been retained.

![Sensor synchronization parameter screen. Check the sensor selection, synchronization status, conveyor type, and input signals.](../../_assets/image30.png)

{% hint style="warning" %}
The resolution, speed, and signal numbers shown on the screen are examples for explanation, not recommended values for your equipment. Do not disable system error detection or arbitrarily increase the allowed pulse anomaly detection count or allowable speed to avoid input abnormalities or speed errors. Investigate the cause first.
{% endhint %}

## Completion Criteria

- The limit switch and pulse counter signal numbers match the actual I/O assignments.
- The communication method and counter type have been checked against the actual board and encoder specifications.
- The saved values are retained when the settings are reopened.

For details, see [3.3 Sensor Synchronization Parameters](../3-user-interface/3-3-sensor-sync-parameter.md).

[__SOURCE](1-intro/1-6-getting-started/2-check-inputs.md)
# 1.6.2 Tutorial - Checking Limit Switch and Encoder Inputs

## Objective and Prerequisites

Verify that the controller receives the workpiece detection signal and encoder pulses. Complete [Basic Settings](1-basic-settings.md), and perform these checks under safe inspection conditions approved by the personnel responsible for the equipment. Do not run the robot program.

## 1. Open the Inspection Screen

1. Open `[Monitoring] > [Sensor Synchronization]`.
2. Select the sensor used in the basic settings.
3. Check the current values of **Limit Switch Input** and **Raw Pulses**.

![Sensor synchronization monitoring screen](../../_assets/image21.png)

## 2. Check the Limit Switch

1. Verify that **Limit Switch Input** is `0` when the limit switch is not actuated.
2. Actuate the actual limit switch using a safe method approved by the personnel responsible.
3. Verify that the input changes to `1` and returns to `0` when the switch is released.

**Expected result:** The limit switch input on the screen matches the state of the actual limit switch.

{% hint style="info" %}
The `[Actuate Limit Switch]` button on the monitoring screen and the `cv.input` command in a program manually register workpiece entry. Do not use them in this step, which checks the actual wiring. Using actual and manual inputs together may cause duplicate workpiece entries to be registered.
{% endhint %}

## 3. Check the Encoder Input

1. Verify that the equipment is in a safe state with no interference between the robot and workpiece.
2. Move the conveyor a short distance at the approved inspection speed. Do not press or hold the workpiece with your hand or a tool.
3. Verify that **Raw Pulses** consistently increase or decrease as the conveyor moves, and record the direction of change.
4. Stop the conveyor and check that the value stabilizes. Do not proceed to the next step if the value continues to change while stopped or changes irregularly during movement.

**Expected result:** The counter changes during movement and stabilizes when stopped. A transition from `ffff` to `0`, or from `0` to `ffff`, may be a wraparound of the 16-bit counter. Do not conclude that the input is abnormal based on a single change in the value.

This step only checks whether the inputs are functioning correctly. Their correspondence to the actual travel direction and distance will be checked after setting the resolution and angle.

## Completion Criteria

- The limit switch input on the screen matches the operation of the actual limit switch.
- Raw pulses change consistently as the conveyor moves.
- Raw pulses are stable when stopped, and the relationship between the travel direction and the direction of counter change has been checked.

## Symptoms and Checks

| Symptom | Check Sequence |
| --- | --- |
| Limit switch input does not change | Selected sensor → limit switch input number → corresponding input in the general I/O monitor → sensor power and wiring |
| Raw pulses do not change | Selected sensor → starting number of the 16-bit input → I/F board status and encoder connection → encoder power and output |
| The value continues to change even when stopped | Have the personnel responsible check for duplicate signal assignments, board settings, wiring, and noise |

{% hint style="warning" %}
Wiring and electrical signal checks must be performed by the personnel responsible in accordance with safety procedures.
{% endhint %}

For details, see [2.2 Hardware Inspection](../2-system-config-access/2-2-hardware-inspection.md) and [3.4 Monitoring](../3-user-interface/3-4-monitoring.md).

[__SOURCE](1-intro/1-6-getting-started/3-calibration.md)
# 1.6.3 Tutorial - Setting the Angle and Encoder Resolution

## Objective and Prerequisites

Configure the controller with the conveyor's travel direction and the pulse count corresponding to its actual travel distance. Complete [1.6.2 Checking Limit Switch and Encoder Inputs](2-check-inputs.md) before proceeding.

Choose **the same repeatably identifiable reference point** on the workpiece for all measurements. Using a different corner or a different workpiece at the first and second positions will introduce calculation errors. Check the measuring tool/TCP, and ensure that the workpiece will not slip on the conveyor.

{% hint style="warning" %}
Move the tool safely clear before moving the conveyor, and stop the conveyor before moving the robot to the reference point. Do not move the conveyor while the tool is in contact with the workpiece reference point. First ensure sufficient safe space and robot reach. If this cannot be ensured, do not arbitrarily use a short measurement distance and treat the result as acceptable; consult the personnel responsible about an alternative measurement method.
{% endhint %}

## 1. Calculate the Conveyor Travel Direction (Angle)

1. Select a new test program for angle calculation and record its number. Keep it separate from existing job programs.
2. With the conveyor stopped, move the tool tip to the workpiece reference point and record `S1`.
3. Move the tool safely clear, then move the conveyor in its travel direction and stop it.
4. Move the tool tip to **the same reference point on the same workpiece** and record `S2`. The order of the two points must match the conveyor's direction of travel.
5. Select the sensor to use under `[Settings] > [Application Parameters] > [Sensor Synchronization]`.
6. Select `[Angle Setting] > [Automatic Calculation]`.
7. Enter the number of the program in which `S1` and `S2` were recorded, and perform the calculation.
8. Check the calculated result and press `[OK]` to save it. Reopen the screen to verify that the value has been retained.

![Recording the reference point at the first position](../../_assets/image22.png)

![Recording the same reference point after moving the conveyor](../../_assets/image23.png)

**Check:** The program number, the order in which the two points were recorded, use of the same reference point and tool/TCP, and whether the calculated result has been saved.

For detailed procedures, see [3.1.1 Program Teaching](../../3-user-interface/3-1-conveyor-angle-auto-set/1-program-teaching.md) and [3.1.2 Performing Automatic Calculation](../../3-user-interface/3-1-conveyor-angle-auto-set/2-auto-calculation.md).

## 2. Calculate the Encoder Resolution

For a linear conveyor, the resolution unit is **pulse/m**. For example, if 10000 pulses are generated over 1 m of travel, the resolution is 10000 pulse/m.

1. Click `[Calculate Resolution]` on the sensor synchronization parameter screen.
2. Allow the workpiece to pass the actual limit switch, then stop the conveyor safely. Check the sensor to be used on the calculation screen.
3. Select position `1`. Move the tool tip to the workpiece reference point and press `[Set Pose]` to record the robot position and pulse count.
4. Move the tool safely clear, then operate the conveyor to move the workpiece and stop it.
5. Select position `2`. Move the tool tip to **the same reference point on the same workpiece** and press `[Set Pose]`.
6. Press `[Calculate Resolution]` and record the calculated value.
7. Repeat the measurement (up to four times) and compare the values. If the calculated values differ significantly, recheck the reference point, slippage, and input status before calculating an average.
8. After verifying the valid measurements, apply the settings by pressing `[Calculate Average]` followed by `[Finish]`. Check and record the saved resolution on the sensor synchronization parameter screen.

![Automatic resolution calculation screen](../../_assets/image27.png)

For the detailed procedure, see [3.2 Automatic Encoder Resolution Setting](../../3-user-interface/3-2-encoder-resolution-auto-set.md).

## 3. Compare Against Actual Travel

1. Place the tool in a safe position and, with the robot program stopped, open `[Monitoring] > [Sensor Synchronization]`.
2. Use a workpiece detected by the actual limit switch. With the conveyor stopped, record the **Workpiece Position** and the actual location of the reference point.
3. Move the conveyor under approved conditions and stop it. Compare the actual travel distance between the two locations with the change in workpiece position on the monitor. Do not measure during movement.
4. Verify that the change in position corresponds correctly to the travel direction and distance. Compare the converted **Workpiece Position (mm)**, not the raw pulses.
5. Record whether the result meets the predefined distance error criteria. Before and after verifying the resolution, also check that the **Travel Speed (mm/s)** matches the actual test conditions.

## Completion Criteria

- The calculated angle and resolution have been saved for the sensor to be used.
- The actual travel direction corresponds to the position change on the monitor.
- The actual travel distance and the change in workpiece position agree within the predefined tolerance.

## Symptoms and Checks

{% hint style="warning" %}
Do not arbitrarily change the sign or magnitude of a value before identifying the cause.
{% endhint %}

| Symptom | Checks |
| --- | --- |
| Incorrect direction | Check the sensor selection, `S1/S2` order, counter type, and wiring |
| Incorrect distance ratio | Check the linear/circular conveyor selection, pulse/m units, reference point, tool/TCP, and encoder input |
| The value continues to change even when stopped | Check for duplicate signal assignments, board settings, wiring, and noise |

[__SOURCE](1-intro/1-6-getting-started/4-teach-and-program.md)
# 1.6.4 Tutorial - Teaching and Creating a Single-Workpiece Program

## Objective and Prerequisites

Teach distinct waiting positions, a synchronized section, and an exit position, and create a program for one workpiece. Complete [Setting the Angle and Encoder Resolution](3-calibration.md), and check the tool/TCP and test area to be used.

{% hint style="warning" %}
This tutorial only checks tracking and exit motion, without activating process outputs. Do not use a production program containing painting, welding, or gripper outputs as-is.
{% endhint %}

## 1. Define the Positions to Teach

First, agree on the following positions with the personnel responsible and record them in the test program.

| Step | Role | Items to Check |
| --- | --- | --- |
| S1 | Safe start position | No interference with the conveyor or workpiece |
| S2 | Approach position | A safe path to the interlock waiting position is available |
| S3 | Interlock waiting position | Near the entry position of the synchronized section, without obstructing workpiece entry |
| S4 | Entry position of the synchronized section | A safe workpiece approach path and sufficient tracking margin are available |
| S5 | Short synchronized work position | A section in which the relative position between the workpiece and tool is maintained |
| S6 | Final position of the synchronized section | Exit is possible without interference after synchronization ends |
| S7 | Safe exit position | The path after synchronization ends is safe |

1. Stop the conveyor and place the test workpiece at the teaching reference position. Record the reference position and the workpiece position relative to the limit switch.
2. Verify that the sensor being used is configured for conveyor synchronization. As described in [3.5 Step Data](../../3-user-interface/3-5-step-data.md), recording a position while sensor synchronization is enabled records the workpiece position along with the robot position. On the actual settings screen, select `Conveyor` as the synchronization type.
3. Record the steps using the same tool/TCP and workpiece reference. A simple, short linear section is recommended for the linear conveyor synchronized section `S4 ~ S6`; use `L` interpolation.
4. In the position properties of each synchronized step, verify that the workpiece position for the sensor being used is recorded in the `ss#` field.
5. Check for interference and reachability along the entire path, including the start, approach, and exit sections. Shifts caused by conveyor movement may make the actual playback positions differ from the taught positions.

## 2. Specify the Waiting Distance

The waiting distance in `cv.wait posi=...` is **the distance the workpiece has traveled from the limit switch**. For a linear conveyor, the unit is mm.
Confirm the actual limit switch location, teaching reference position, and safe entry area with the personnel responsible before setting the value. The workpiece continues to move while the robot enters after the waiting distance is reached, so check both the entry path and the available tracking margin.

## 3. Write the Program

```hrscript
    global cv
    cv=sync.Sensor(1)   # Sensor object to use
    cv.sync reset   # End synchronization and initialize conveyor data before actual workpiece entry
S1  move P,spd=100%,accu=1,tool=1 
S2  move P,spd=30%,accu=5,tool=1  # Move to the synchronization waiting position
    delay 0.5
    cv.wait posi=200,sync=off   # Wait for limit switch detection and workpiece travel to the specified position
    cv.sync on  # Start the synchronized section
S3  move L,spd=30%,accu=1,tool=1  
    delay 1
S4  move L,spd=30%,accu=1,tool=1  
    delay 1
S5  move L,spd=30%,accu=1,tool=1  
    delay 1
S6  move L,spd=30%,accu=1,tool=1  
    delay 3
    cv.sync off # End the synchronized section
S7  move P,spd=100%,accu=1,tool=1  # Move to the end position
    end
```

## Completion Criteria

- The roles of S1 ~ S7, the synchronized section S4 ~ S6, and the exit path are clearly identified.
- The sensor position data, interpolation method, and tool data of the synchronized steps have been checked.
- The relationship between the teaching reference and waiting distance has been recorded.
- Preparations ensure that one actual workpiece enters after initialization.
- The actual program syntax, steps, and command placement have been checked with the personnel responsible.

[__SOURCE](1-intro/1-6-getting-started/5-first-run.md)
# 1.6.5 Tutorial - First Test Run and Restart

## Objective and Prerequisites

Use one workpiece to verify waiting, synchronized tracking, the end of synchronization, and a safe exit. Proceed only after meeting all completion criteria in [Teaching and Program Structure](4-teach-and-program.md).

{% hint style="warning" %}
Perform the test in the presence of the personnel responsible and in accordance with the site's safe operating procedures. Do not assume that simply reducing the robot speed will allow it to track the conveyor. Consider both the conveyor speed and the robot's tracking capability. Do not bypass guards or interlocks, or enter the workspace during operation to check positions. If motion is hazardous or unexpected, use the site's stopping procedure. `cv.sync off` and a manual reset are not substitutes for a safe stop.
{% endhint %}

## 1. Pre-Start Checks

- The path from the robot's current position to the start step and the exit path are safe.
- There is one test workpiece, and it has not yet passed the limit switch.
- The test sequence has been checked to ensure that data for workpieces detected before initialization will not continue to be used.
- The conveyor and robot test speeds, test area, and tracking error acceptance criteria have been confirmed with the personnel responsible.
- Process outputs will not activate, and the monitoring screen can be viewed from outside the workspace.

## 2. Test Procedure

1. Open `[Monitoring] > [Sensor Synchronization]` and check the sensor to be used.
2. Start the program under approved test conditions. Arrange the equipment sequence so that the workpiece enters after initialization. If the order of initialization and actual entry cannot be guaranteed, do not start; have the personnel responsible improve the interlocks.
3. With the robot at the waiting position, introduce one workpiece so that the actual limit switch detects it. Introduce the workpiece from outside the workspace using an approved method.
4. On the monitoring screen, check the actual limit switch input, the number of entered workpieces, and the workpiece position. Do not also use manual limit switch input or `cv.input`.
5. Verify that the robot waits until the workpiece reaches the specified waiting distance.
6. From a safe position, observe whether the relative position and orientation between the tool and workpiece are maintained in the synchronized section, or verify this using an approved measurement method.
7. Verify that, after synchronization ends, the robot moves safely to the end position and the program terminates.

## 3. Expected Results at Each Stage

| Stage | Expected Behavior | Checks if Abnormal |
| --- | --- | --- |
| After initialization, before actual entry | Existing workpiece data is cleared | Check the sensor targeted for reset and the external reset input |
| When the workpiece passes the limit switch | The limit switch input changes and one workpiece is detected | Check the input status and whether manual input is also being used |
| During conveyor movement | The workpiece position (mm) changes in accordance with movement | Recheck the resolution and input signals |
| While waiting | The robot does not enter the synchronized section before the waiting distance is reached | Check the waiting distance and the command being executed |
| In the synchronized section | The tracking direction is correct, and relative position and orientation remain within the defined criteria | Stop the test and check the angle, teaching data, and tracking capability |
| Completion and exit | After the synchronized section ends, the robot moves to a safe position without interference | Recheck the position of the synchronization end command and the exit path |

If checking the configured synchronization ON output through external I/O, also verify the output assignment.

## 4. Restart Procedure After an Interruption

Resuming directly from an intermediate step may change the relationship between the workpiece data and the robot position. For this initial exercise, repeat the test **from the beginning** using the following procedure.

1. Stop the robot and conveyor safely according to site procedures, and verify that they have stopped.
2. Record the errors and monitored values, and resolve the cause. Refer to [Troubleshooting](6-troubleshooting.md).
3. Clear any remaining workpieces according to safety procedures. Do not resume work on a workpiece whose tracking data has been cleared.
4. With the program stopped, initialize the sensor data using `[Manual Reset]` if necessary. This operation resets pulses, workpiece positions, entry counts, synchronization status, and other data, so first check the target sensor and the actual workpiece state.
5. Place the robot at a safe start position using approved manual operation procedures, and recheck the travel path.
6. Prepare a new test workpiece at a position before it passes the limit switch, and run the program from the beginning. The actual workpiece must enter after initialization within the program.

## Final Completion Checklist

- Only one workpiece is detected through the actual input.
- The robot waits for the specified distance and performs synchronization in the intended section.
- The tracking direction and relative position and orientation meet the criteria.
- The end of synchronization and a safe exit have been verified.
- The procedure for repeating the test from the beginning after an interruption has been verified.
- The verified program, settings, test conditions, and results have been saved.

[__SOURCE](1-intro/1-6-getting-started/6-troubleshooting.md)
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

[__SOURCE](2-system-config-access/README.md)
# 2. System Configuration and Connections

The system configuration required to use conveyor synchronization is as follows.

![](../_assets/image15.png)

[__SOURCE](2-system-config-access/2-1-conveyor-if-board.md)
# 2.1 Conveyor I/F Board

The conveyor I/F boards supported by our company are as follows. Please refer to separate documentation for details.

- **M5112**

    Use the Crevis FnIO module combined with a Network Adapter and Power Module.

    * Manual:
      https://www.crevis.ru/files/spec/g/[Spec]%20M5112%20(Rev%201.03).pdf

[__SOURCE](2-system-config-access/2-2-hardware-inspection.md)
# 2.2 Hardware Inspection

Select **[Monitoring > Sensor Sync]** to check sensor sync related data.

Before inspection, complete the [Input Signal Settings](../3-user-interface/3-3-sensor-sync-parameter.md) for the sensor to be used. If the signal assignments are incorrect, the correct values will not be displayed on the monitor even if the wiring is normal.

Select `[Monitoring > Sensor Sync]` and select the sensor to inspect to view the data related to sensor synchronization.

![](../_assets/image21.png)

- **Limit switch**

    The "Limit switch input" field shows 1 when the limit switch is active and 0 when it is not. If it does not operate normally, inspect the hardware.

- **Encoder**

    The "raw pulse" field shows encoder pulses; during conveyor movement the value should continuously increase or decrease within the range 0~ffff. If not operating normally, inspect the hardware.

[__SOURCE](3-user-interface/README.md)
# 3. User Interface

[__SOURCE](3-user-interface/3-1-conveyor-angle-auto-set/README.md)
# 3.1 Conveyor Angle Auto-Set

If the conveyor direction is placed arbitrarily, measuring the conveyor movement accurately in 3D space can take considerable time. Therefore, the robot controller must know in advance the direction of conveyor motion in the robot coordinate frame for robot synchronization.  
Use the controller's built-in auto-angle calculation feature for this.

[__SOURCE](3-user-interface/3-1-conveyor-angle-auto-set/1-program-teaching.md)
# 3.1.1 Program Teaching

To perform conveyor angle auto-calculation, first write a program as follows.

{% hint style="info" %}  
To set the angle accurately, make each recorded position as far apart as possible (recommended at least 1 m for straight conveyors).
{% endhint %}


1. Select a new program for conveyor angle auto-calculation.
2. Move the robot tool tip to a specific position on the conveyor workpiece and record S1.

![](../../_assets/image22.png)

3. Move the conveyor to shift the workpiece, move the robot tool tip to the same specific position and record S2.

![](../../_assets/image23.png)

4. A program similar to the following will be created.

![](../../_assets/image24.png)


{% hint style="info" %}    
For circular conveyors, three positions are required to calculate the angle. Repeat step 3 once more.  
{% endhint %}
[__SOURCE](3-user-interface/3-1-conveyor-angle-auto-set/2-auto-calculation.md)
# 3.1.2 Run Auto-Calculation

In the [Settings > Application Parameters > Sensor Sync] screen, press `[Angle Setting]` to display the angle screen.

{% hint style="info" %}  
If the conveyor type is set to circular, the settings below are modified to define the angle and center of the circular conveyor.
{% endhint %}

![](../../_assets/image25.png)

1. Check or manually set the currently configured conveyor angle.
2. For auto-calculation, press `[Auto Calculate]`, enter the taught program number and the calculation result will be shown.

![](../../_assets/image26.png)

3. Press [OK] to save the configured values.

[__SOURCE](3-user-interface/3-2-encoder-resolution-auto-set.md)
# 3.2 Encoder Resolution Auto-Set

Encoder resolution means the number of pulses generated when a linear conveyor (or press) moves 1 m, or when a circular conveyor (or press) rotates 1 degree.

To automatically calculate encoder resolution, go to `[Settings > Application Parameters > Sensor Sync]` and press `[Resolution Calculation]`.

![](../_assets/image27.png)

1. After the workpiece triggers the limit switch, designate the sensor as shown below.

![](../_assets/image28.png)

2. Select position <1>.
3. Move the robot tool tip to a specific position on the workpiece.
4. Press `[Record Pose]` to record the current robot pose and the encoder pulse value.
5. Move the workpiece (at least 1 m) by operating the sensor as shown.

![](../_assets/image29.png)

6. Select position <2>.
7. Move the robot tool tip back to the specific position recorded in step 3.
8. Press `[Record Pose]` to record the pose and encoder pulse value.
9. Press `[Calculate Resolution]` to compute the encoder resolution and record it in the encoder resolution field.

Repeat steps 1~9 to calculate up to four encoder resolutions.

10. Press `[Calculate Average]` to compute the average of the recorded encoder resolutions.
11. Press `[OK]` to set the average as the encoder resolution.

[__SOURCE](3-user-interface/3-3-sensor-sync-parameter.md)
# 3.3 Sensor Sync Parameters

To apply conveyor (press) synchronization and execute robot motion, the robot controller must know various information about the conveyor (press) it must synchronize with. These parameters must be set before writing the operation program.

{% hint style="warning" %}
The numerical values shown on the screen are examples and are not recommended values for each piece of equipment. To avoid errors, do not arbitrarily increase the allowable speed or the allowed number of pulse anomaly detections, nor disable system error detection related to synchronization. You must first check the actual operating conditions and input signals.
{% endhint %}

![](../_assets/image30.png)

- **Conveyor form**

    Select the form according to the figure below.

![](../_assets/image31.png)

- **Encoder resolution**

    Encoder resolution is defined as the number of pulses generated when a linear conveyor moves 1 m or when a circular conveyor rotates 1 degree.

{% hint style="info" %}
See section [3.2 Encoder Resolution Auto-Set](3-2-encoder-resolution-auto-set.md) for automatic encoder resolution calculation.
{% endhint %}

- **Conveyor allowable speed**

    This parameter is used to treat abnormally high speeds as errors. Configure it considering the expected operating speed; the controller internally calculates conveyor speed and reports an error if the speed exceeds the configured allowable speed.

{% hint style="info" %}  
Encoder pulses typically have ripple around their average, so speed also shows slight ripple. Set the allowable speed slightly higher to accommodate this.
{% endhint %}

- **Allowed count for pulse anomaly detection**

    If pulses are abnormally input, the robot controller outputs the error "E0019 Conveyor pulse allowed frequency exceeded". Configure this to allow the robot to continue working (protecting the workpiece) by tolerating a certain number of pulse anomalies during sync operation.

{% hint style="info" %}  
For example, if the allowable number of pulse anomaly detections is set to 3, the robot controller does not generate an error even if pulse anomalies are detected up to three times for a single workpiece during synchronized operation, and instead internally generates appropriate pulse values. When a fourth pulse anomaly is detected, an error is generated. Information on the number of detected pulse anomalies is reset when playback for the corresponding workpiece is completed.
{% endhint %}  

- **Maximum allowed number of workpieces**

    Configure whether the robot should continue working when another workpiece triggers the limit switch and enters the workspace while the robot is synchronizing a workpiece. The maximum allowed is 100.

- **Detect sync-related system errors**

    If system installation is incomplete or a board is damaged causing sync-related system errors that prevent switching to RUN READY, configure to ignore sync-related system errors so unrelated operations can continue.

| **Error No.** | **Sync system error type** |
| :-----------: | ------------------------- |
| E0021 | Conveyor allowable speed exceeded |

- **Sync reset input**

    External input can clear conveyor (press) data. When this signal is input while the robot is stopped, sensor-related data (pulse data, workpiece positions, speed, number of workpieces, sync playback state, etc.) are cleared, equivalent to a manual reset.

- **Limit switch input**

    Accept limit switch state via external input.

- **Pulse counter input**

    Accept encoder pulse counter via external input. The controller manages the pulse counter internally as 16-bit data, so a 1-word (2-byte) input signal is used. Specifying the lowest bit signal number automatically assigns 16 consecutive signals.

![](../_assets/image32.png)

- **Pulse line error**

    When using line drive pulse communication, you can detect open circuits on the pulse line. Use this to report pulse line errors to the robot controller.

- **Conveyor sync ON**

    Conveyor sync state can be output externally. When the command **"cv.sync start"** runs and sync is ON, it outputs **"1"**.

- **Pulse counter type**

    When the conveyor moves forward, pulse values increase. If pulses increase when moving backward, that is the "Up" type; if they decrease, use the "Up/Down" type. Typically use "Up/Down". On our board, "Up" is output as off, and "Up/Down" as on.

- **Pulse communication type**

    Typical pulse communication uses open collector and line drive. Refer to separate learning materials for details. On our board, "line drive" is off and "open collector" is on.

[__SOURCE](3-user-interface/3-4-monitoring.md)
# 3.4 Monitoring

Select **[Monitoring > Sensor Sync]** to view sensor sync related data. Use **[sensor sync. operate]** buttons to perform various actions.

![](../_assets/image33.png)

- **Pulse data**

    The number of pulses counted for the workpiece since the limit switch.

- **Workpiece position**

    The distance the workpiece has moved from the limit switch. For <linear> form it is in mm; for <circular> form it is in degrees.

- **Velocity**

    The movement speed of the conveyor (or press). For <linear> form it is mm/s; for <circular> form it is deg/s.

- **Number of entered workpieces**

    The number of workpieces that have triggered the limit switch and entered.

- **Limit switch input**

    Shows whether the limit switch is active.

- **raw pulse**

    Displays the encoder pulse counter value as hex data (0~ffff) during normal operation.

- **Manual reset**

    Manually clears sensor-related data (pulse data, workpiece positions, speed, number of workpieces, sync playback state, etc.).

- **Enter workpiece position**

    Manually input sensor position values (mm for linear, deg for circular).

- **Limit switch operation**

    Use when you need to manually toggle the limit switch.

[__SOURCE](3-user-interface/3-5-step-data.md)
# 3.5 Step Data

When sensor sync is set to <enabled> and you press [Record], the current robot axis positions along with the current workpiece position are recorded as shown below.

The robot uses the recorded position data during conveyor sync playback.

![](../_assets/image34.png)

You can view and edit the recorded workpiece position in the ss# field of the current step position properties.

![](../_assets/image35.png)

[__SOURCE](3-user-interface/3-6-command.md)
# 3.6 Commands

- **cv.sync (sync playback)**

    ### Description:

    Specifies the section to execute sensor sync during program playback.

    ### Syntax:

    ```python
    cv.sync <sync_action>
    ```

    ### Parameters
    <table>
    <thead>
        <tr>
            <th style="text-align:left">Item</th>
            <th style="text-align:left">Description</th>
            <th style="text-align:left">Remarks</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td style="text-align:left">Synchronization Operation</td>
            <td style="text-align:left">
                - reset: Reset sync. (Sync OFF + clear conveyor data)<br>
                - on: Start sync. (Sync ON)<br>
                - off: Pause sync. (Sync OFF)<br>
                - next: Start sync. for the next workpiece (Sync OFF + load next workpiece data)<br>
            </td>
            <td style="text-align:left">String</td>
        </tr>
    </tbody>
    </table>


- **cv.wait (interlock wait)**

    ### Description:

    Use to pause the robot until the workpiece reaches a specified position from the limit switch.

    ### Syntax:
    ```python
    cv.wait posi=<wait_distance>,sync=<sync_flag>
    ```

    ### Parameters:
    - wait_distance: Distance from the limit switch to wait for the workpiece (variable)
    - sync_flag: 0 = asynchronous, 1 = synchronous (not supported for press)

- **cv.input (workpiece entry)**

    ### Description:

    Use when the limit switch triggers to register that one workpiece has entered.

    ### Syntax:
    ```
    cv.input
    ```

### Example

This example explains a structure that manually recognizes workpiece entry using `cv.input`. If added as-is to a program that uses actual limit switch inputs, it may result in duplicate recognition. Robot coordinates and sensor position data must be taught, and the speed, waiting distance, and tool number in the example are not recommended values.

```python
    global cv
    cv=sync.Sensor(1)
    cv.sync reset
S1  move P,spd=100%,accu=1,tool=1 
S2  move P,spd=30%,accu=5,tool=1  
    delay 0.5
    cv.input
    cv.wait posi=200,sync=off
    cv.sync on
S3  move L,spd=30%,accu=1,tool=1  
    delay 1
S4  move L,spd=30%,accu=1,tool=1  
    delay 1
S5  move L,spd=30%,accu=1,tool=1  
    delay 1
S6  move L,spd=30%,accu=1,tool=1  
    delay 3
    cv.sync off
S7  move P,spd=100%,accu=1,tool=1  
    end
    ```
[__SOURCE](3-user-interface/3-7-function.md)
# 3.7 Functions

- **cv.position(<workpiece_index>)**

    Use `cv.position` to obtain the current position of a workpiece.

    ### Description
    When multiple workpieces sequentially pass the limit switch, use this to get the distance moved (mm) from the limit switch for each workpiece. Index 0 corresponds to the first-entered workpiece. Index numbers increase in order of entry.

    ### Syntax
    ```python
    result = cv.position(<workpiece_index>)
    ```

    ### Parameters
    <table>
    <thead>
        <tr>
        <th style="text-align:left">Item</th>
        <th style="text-align:left">Description</th>
        <th style="text-align:left">Remarks</th>
        </tr>
    </thead>
    <tbody>
        <tr>
        <td style="text-align:left">Workpiece Index</td>
        <td style="text-align:left">
            Matched sequentially starting from 0 according to the order in which workpieces enter
        </td>
        <td style="text-align:left">Variable</td>
        </tr>
    </tbody>
    </table>


    ### Example
    ```python
    if cv.position(0) > 1000 then
        print "Workpiece has left the allowable work area."
    endif
    ```
[__SOURCE](3-user-interface/3-8-variable.md)
# 3.8 Variables

- **cv.speed (conveyor speed)**

    ### Description
    Use to read the movement speed of a conveyor or press; corresponds to the monitoring velocity.

    ### Example
    ```python
    if cv.speed > 300 then
        print "Conveyor speed is too high."
    endif
    ```

- **cv.pulse (workpiece pulses)**

    ### Description
    Use to read the pulses (distance) the workpiece has moved from the limit switch; corresponds to monitoring pulse data.

    ### Example
    ```python
    if cv.pulse > 10000 then
        print "Workpiece has left the allowable work area."
    endif
    ```

- **cv.position (workpiece position)**
    ### Example
    ```python
    if cv.position > 1000 then
        print "Workpiece has left the allowable work area."
    endif
    ```

- **cv.work_no (number of entered workpieces)**

    ### Description
    Use to read the number of workpieces that have entered the conveyor after passing the limit switch; corresponds to the monitoring entered workpiece count.

    ### Example
    ```python
    if cv.work_no > 30 then
        print "Exceeded allowed number of entered workpieces."
    endif
    ```

- **cv.raw_pulse (encoder raw pulse)**

    ### Description
    Use to read the current pulse counter input from the encoder; corresponds to monitoring raw pulse.

    ### Example
    ```python
    var raw_pulse = cv.raw_pulse
    ```

- **cv.resolution (encoder resolution)**

    ### Description
    Use to read the encoder resolution set by the user.

    ### Example
    ```python
    var resolution = cv.resolution
    ```
[__SOURCE](4-teaching/README.md)
# 4. Teaching

Writing programs for sensor synchronization follows the same general teaching workflow. However, to execute sensor sync playback you must use the commands [cv.sync](../3-user-interface/3-6-command.md) (sync playback) and [cv.wait](../3-user-interface/3-6-command.md) (sensor interlock wait); these commands must be recorded in the taught program before playback.

[__SOURCE](4-teaching/4-1-sync-oper-program-config.md)
# 4.1 Sync Operation Program Structure

- **Home position wait**

    The robot waits at its home position until a start command is input.

- **Interlock wait**

    The robot moves near the synchronization section and waits until the workpiece reaches the distance recorded by `cv.wait`.

The following figure shows a painting program for workpieces flowing on a conveyor. The robot starts conveyor sync when advancing to step S4 and begins spraying paint in sync from step S5. The interlock wait step (S3) is recorded near the sync section entry step (S4).

![](../_assets/image_1.png)

Example program:

```python
    global cv
    cv = sync.Sensor(1)
    cv.sync reset               # Conveyor sync reset
S1                              # Robot home
S2
S3                             # Interlock wait step
    cv.sync on                 # Start conveyor sync
    cv.wait posi=500,sync=0    # Conveyor interlock wait
S4                             # Sync section entry step
    do1 = 1                    # Paint spray ON signal
S5                             # First sync operation step
 :
S9                             # Last sync operation step
    do1 = 0                    # Paint spray OFF signal
    cv.sync off                # End conveyor sync
                                # Complete current work
S10
 :
S13                            # Robot home
    end
```

- **Sync playback**

    In the figure, the conveyor sync playback section refers to steps S4 through S9; all commands in this section are executed synchronized to the moving conveyor.

- **Return to home position**

    After finishing the operation, the robot returns to its home position for the next start command.

[__SOURCE](4-teaching/4-2-press-sync-teaching.md)
# 4.2 Press Sync Teaching

Press synchronization makes the robot follow the press speed. The press speed is assumed to be constant; if the press speed varies, synchronization performance degrades. Set the current press allowable speed in the sensor sync parameter settings under **"Allowed Speed"**.

Example program using press sync:

```python
    global press
    press = sync.Sensor(1)
    press.sync reset              # Press sync reset
S1
    press.sync on                 # Start press sync
    press.wait posi=500,sync=0    # Press interlock wait
S2  move P,spd=60%                # Record position for sensor 1
S3  move P,spd=60%                # Record position for sensor 1
S4  move P,spd=60%                # Record position for sensor 1
    press.sync off                # End press sync
S5
    end
```

In the above program, the sensor-recorded positions at steps 2, 3, and 4 must strictly increase; otherwise the following error occurs:

| **Error Code** | **Error Message** |
| :------------: | ----------------- |
| E0239          | Step sensor positions are not strictly increasing. |

Additionally, the speeds recorded at steps 2, 3, and 4 are ignored; the motion is planned based on the user's configured allowable press speed. If the recorded sensor and robot positions require motion exceeding robot capability even when planned at maximum speed, the following error occurs during operation:

| **Error Code** | **Error Message** |
| :------------: | ----------------- |
| E0238          | Cannot follow the sensor speed. |

[__SOURCE](5-faq.md)
# 5. Frequently Asked Questions

## Initial Setup & Testing
- **The limit switch or raw pulse does not change.**
    Check the selected sensor and input signal assignment first, then follow the inspection sequence in Tutorial - Checking Input Signals.

- **The workpiece moves, but the program keeps waiting.**
    Verify whether the actual workpiece is recognized, check the workpiece position and waiting distance on the monitor, and ensure it resets after entry. Refer to Tutorial - Troubleshooting.

- **Can I use the speed, resolution, and waiting distance values from the example as they are?**
    No. The values shown on the screen and in the program are examples only. You must verify the actual equipment's moving direction and resolution, and determine values that match your teaching baseline and operating conditions.

- **Can I resume from an intermediate step after stopping a test?**
    For your first practice run, clear the workpiece data, reset the robot position, and start the test from the beginning of the program. Refer to Tutorial - Restarting After Interruption.
 
## Other Operational Questions
- **If an additional axis has base specifications and the axis configuration is linear, how does conveyor sync operate?**

    When an auxiliary axis exists during conveyor sync, the robot first follows the workpiece using the additional axis. If the robot cannot follow with the auxiliary axis due to soft limits or arm interference, it uses the robot's 6 axes to follow the workpiece.

- **What happens if the B-axis angle passes near 0 degrees during conveyor sync?**

    If the B-axis passes near 0 degrees during conveyor sync, the robot cannot keep the tool orientation stable. When mounting the tool, choose a tool orientation that avoids B-axis angles near 0 degrees.

- **How can I manually input the limit switch?**

    Use the **[Limit Switch Operation]** button in Sensor Sync Monitoring.

- **How can I manually clear current conveyor (press) data?**

    Use the [Manual Reset] button in Sensor Sync Monitoring.
