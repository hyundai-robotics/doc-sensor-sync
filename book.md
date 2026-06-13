
[__SOURCE](README.md)
# ${cont_model} Controller Function Manual - Sensor Synchronization (Conveyor, Press)

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

![](../_assets/image12.png)

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

In the [Settings > Application Parameters > Sensor Sync] screen, press [Angle Setting] to display the angle screen.

{% hint style="info" %}  
If the conveyor type is set to circular, the settings below are modified to define the angle and center of the circular conveyor.
{% endhint %}

![](../../_assets/image25.png)

1. Check or manually set the currently configured conveyor angle.
2. For auto-calculation, press [Auto Calculate], enter the taught program number and the calculation result will be shown.

![](../../_assets/image26.png)

3. Press [OK] to save the configured values.

[__SOURCE](3-user-interface/3-2-encoder-resolution-auto-set.md)
# 3.2 Encoder Resolution Auto-Set

Encoder resolution means the number of pulses generated when a linear conveyor (or press) moves 1 m, or when a circular conveyor (or press) rotates 1 degree.

To automatically calculate encoder resolution, go to [Settings > Application Parameters > Sensor Sync] and press [Resolution Calculation].

![](../_assets/image27.png)

1. After the workpiece triggers the limit switch, designate the sensor as shown below.

![](../_assets/image28.png)

2. Select position <1>.
3. Move the robot tool tip to a specific position on the workpiece.
4. Press [Record Pose] to record the current robot pose and the encoder pulse value.
5. Move the workpiece (at least 1 m) by operating the sensor as shown.

![](../_assets/image29.png)

6. Select position <2>.
7. Move the robot tool tip back to the specific position recorded in step 3.
8. Press [Record Pose] to record the pose and encoder pulse value.
9. Press [Calculate Resolution] to compute the encoder resolution and record it in the encoder resolution field.

Repeat steps 1~9 to calculate up to four encoder resolutions.

10. Press [Calculate Average] to compute the average of the recorded encoder resolutions.
11. Press [OK] to set the average as the encoder resolution.

[__SOURCE](3-user-interface/3-3-sensor-sync-parameter.md)
# 3.3 Sensor Sync Parameters

To apply conveyor (press) synchronization and execute robot motion, the robot controller must know various information about the conveyor (press) it must synchronize with. These parameters must be set before writing the operation program.

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

- **If an additional axis has base specifications and the axis configuration is linear, how does conveyor sync operate?**

    When an auxiliary axis exists during conveyor sync, the robot first follows the workpiece using the additional axis. If the robot cannot follow with the auxiliary axis due to soft limits or arm interference, it uses the robot's 6 axes to follow the workpiece.

- **What happens if the B-axis angle passes near 0 degrees during conveyor sync?**

    If the B-axis passes near 0 degrees during conveyor sync, the robot cannot keep the tool orientation stable. When mounting the tool, choose a tool orientation that avoids B-axis angles near 0 degrees.

- **How can I manually input the limit switch?**

    Use the **[Limit Switch Operation]** button in Sensor Sync Monitoring.

- **How can I manually clear current conveyor (press) data?**

    Use the [Manual Reset] button in Sensor Sync Monitoring.
