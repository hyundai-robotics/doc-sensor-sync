# 1.6 Tutorial

This tutorial guides users who are setting up sensor synchronization for the first time through **checking input signals → setting the travel direction and resolution → teaching → performing the first test run**.

### Objective

Using one linear conveyor and one workpiece, verify that the robot tracks the workpiece over a short section and then moves away safely.
This tutorial is intended for **users who have completed basic robot operation and safety training**. If you are new to robot operation, first review the controller operation manual and complete the required safety training.

{% hint style="warning" %}
This tutorial does not replace the equipment's safety procedures. Read the [Safety Precautions](../../0-about-this-manual/safety-notice.md) first, and comply with the site's risk assessment, interlocks, and stopping procedures. Wiring work must be performed by authorized personnel with the power disconnected. Do not touch moving workpieces or operate the robot and conveyor simultaneously from inside the workspace.
{% endhint %}

### Before You Start

- Back up any existing sensor synchronization settings and job programs. Prepare a test program without overwriting an existing production program.
- Have the personnel responsible verify the installation and connections of the supported I/F board, encoder, and limit switch. Refer to [2. System Configuration and Connections](../../2-system-config-access/README.md).
- Prepare a test environment where the conveyor can be moved and stopped safely, and where potential interference between the robot, tool, and workpiece can be checked.
- Verify the tool/TCP and robot coordinate system to be used. Use the same reference in the angle calculation program and the test program.
- Agree with the personnel responsible for the equipment on the initial test speed, acceptance criteria for tracking error, and a safe test area. (The speeds shown in this manual's screens and examples are not recommended values.)

### Procedure Overview

| Step | Task | Requirements for Proceeding |
| --- | --- | --- |
| [1.6.1 Basic Settings](1-basic-settings.md) | Select the sensor and conveyor type, and assign signals | The settings match the actual equipment |
| [1.6.2 Checking Input Signals](2-check-inputs.md) | Check the limit switch and raw pulses | Limit switch input and pulse changes have been verified |
| [1.6.3 Setting the Travel Direction and Resolution](3-calibration.md) | Calculate the angle and resolution, and compare against actual travel | The travel direction and distance have been verified |
| [1.6.4 Teaching and Program Structure](4-teach-and-program.md) | Record the waiting, synchronized, and exit positions, and add commands | The step data and program structure have been checked |
| [1.6.5 First Test Run and Restart](5-first-run.md) | Test with a single workpiece, and check completion and restart | Tracking and a safe exit have been verified |
| [1.6.6 Troubleshooting](6-troubleshooting.md) | Check according to the symptoms | The cause has been resolved and checks have been repeated from the relevant step |

### Key Terms

- **Limit switch:** A sensor that detects workpiece entry. It serves as the reference point for determining the workpiece position.
- **Raw pulses:** The raw pulse count received from the encoder. The monitor displays it in hexadecimal, cycling through the range `0～ffff`.
- **Workpiece position:** The position to which the workpiece has traveled relative to the limit switch. For a linear conveyor, the unit is mm.
- **Encoder resolution:** The number of pulses generated when a linear conveyor travels 1 m.
- **Synchronized section:** A section executed with the robot position adjusted to follow a moving workpiece.
