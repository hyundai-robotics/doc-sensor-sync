# 1.6.4 Tutorial - Teaching and Creating a Single-Workpiece Program

### Objective and Prerequisites

Teach distinct waiting positions, a synchronized section, and an exit position, and create a program for one workpiece. Complete [Setting the Angle and Encoder Resolution](3-calibration.md), and check the tool/TCP and test area to be used.

{% hint style="warning" %}
This tutorial only checks tracking and exit motion, without activating process outputs. Do not use a production program containing painting, welding, or gripper outputs as-is.
{% endhint %}

### 1. Define the Positions to Teach

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

### 2. Specify the Waiting Distance

The waiting distance in `cv.wait posi=...` is **the distance the workpiece has traveled from the limit switch**. For a linear conveyor, the unit is mm.
Confirm the actual limit switch location, teaching reference position, and safe entry area with the personnel responsible before setting the value. The workpiece continues to move while the robot enters after the waiting distance is reached, so check both the entry path and the available tracking margin.

### 3. Write the Program

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

### Completion Criteria

- The roles of S1 ~ S7, the synchronized section S4 ~ S6, and the exit path are clearly identified.
- The sensor position data, interpolation method, and tool data of the synchronized steps have been checked.
- The relationship between the teaching reference and waiting distance has been recorded.
- Preparations ensure that one actual workpiece enters after initialization.
- The actual program syntax, steps, and command placement have been checked with the personnel responsible.
