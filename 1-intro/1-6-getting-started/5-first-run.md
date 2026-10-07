# 1.6.5 Tutorial - First Test Run and Restart

### Objective and Prerequisites

Use one workpiece to verify waiting, synchronized tracking, the end of synchronization, and a safe exit. Proceed only after meeting all completion criteria in [Teaching and Program Structure](4-teach-and-program.md).

{% hint style="warning" %}
Perform the test in the presence of the personnel responsible and in accordance with the site's safe operating procedures. Do not assume that simply reducing the robot speed will allow it to track the conveyor. Consider both the conveyor speed and the robot's tracking capability. Do not bypass guards or interlocks, or enter the workspace during operation to check positions. If motion is hazardous or unexpected, use the site's stopping procedure. `cv.sync off` and a manual reset are not substitutes for a safe stop.
{% endhint %}

### 1. Pre-Start Checks

- The path from the robot's current position to the start step and the exit path are safe.
- There is one test workpiece, and it has not yet passed the limit switch.
- The test sequence has been checked to ensure that data for workpieces detected before initialization will not continue to be used.
- The conveyor and robot test speeds, test area, and tracking error acceptance criteria have been confirmed with the personnel responsible.
- Process outputs will not activate, and the monitoring screen can be viewed from outside the workspace.

### 2. Test Procedure

1. Open `[Monitoring] > [Sensor Synchronization]` and check the sensor to be used.
2. Start the program under approved test conditions. Arrange the equipment sequence so that the workpiece enters after initialization. If the order of initialization and actual entry cannot be guaranteed, do not start; have the personnel responsible improve the interlocks.
3. With the robot at the waiting position, introduce one workpiece so that the actual limit switch detects it. Introduce the workpiece from outside the workspace using an approved method.
4. On the monitoring screen, check the actual limit switch input, the number of entered workpieces, and the workpiece position. Do not also use manual limit switch input or `cv.input`.
5. Verify that the robot waits until the workpiece reaches the specified waiting distance.
6. From a safe position, observe whether the relative position and orientation between the tool and workpiece are maintained in the synchronized section, or verify this using an approved measurement method.
7. Verify that, after synchronization ends, the robot moves safely to the end position and the program terminates.

### 3. Expected Results at Each Stage

| Stage | Expected Behavior | Checks if Abnormal |
| --- | --- | --- |
| After initialization, before actual entry | Existing workpiece data is cleared | Check the sensor targeted for reset and the external reset input |
| When the workpiece passes the limit switch | The limit switch input changes and one workpiece is detected | Check the input status and whether manual input is also being used |
| During conveyor movement | The workpiece position (mm) changes in accordance with movement | Recheck the resolution and input signals |
| While waiting | The robot does not enter the synchronized section before the waiting distance is reached | Check the waiting distance and the command being executed |
| In the synchronized section | The tracking direction is correct, and relative position and orientation remain within the defined criteria | Stop the test and check the angle, teaching data, and tracking capability |
| Completion and exit | After the synchronized section ends, the robot moves to a safe position without interference | Recheck the position of the synchronization end command and the exit path |

If checking the configured synchronization ON output through external I/O, also verify the output assignment.

### 4. Restart Procedure After an Interruption

Resuming directly from an intermediate step may change the relationship between the workpiece data and the robot position. For this initial exercise, repeat the test **from the beginning** using the following procedure.

1. Stop the robot and conveyor safely according to site procedures, and verify that they have stopped.
2. Record the errors and monitored values, and resolve the cause. Refer to [Troubleshooting](6-troubleshooting.md).
3. Clear any remaining workpieces according to safety procedures. Do not resume work on a workpiece whose tracking data has been cleared.
4. With the program stopped, initialize the sensor data using `[Manual Reset]` if necessary. This operation resets pulses, workpiece positions, entry counts, synchronization status, and other data, so first check the target sensor and the actual workpiece state.
5. Place the robot at a safe start position using approved manual operation procedures, and recheck the travel path.
6. Prepare a new test workpiece at a position before it passes the limit switch, and run the program from the beginning. The actual workpiece must enter after initialization within the program.

### Final Completion Checklist

- Only one workpiece is detected through the actual input.
- The robot waits for the specified distance and performs synchronization in the intended section.
- The tracking direction and relative position and orientation meet the criteria.
- The end of synchronization and a safe exit have been verified.
- The procedure for repeating the test from the beginning after an interruption has been verified.
- The verified program, settings, test conditions, and results have been saved.
