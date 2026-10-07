# 1.6.3 Tutorial - Setting the Angle and Encoder Resolution

### Objective and Prerequisites

Configure the controller with the conveyor's travel direction and the pulse count corresponding to its actual travel distance. Complete [1.6.2 Checking Limit Switch and Encoder Inputs](2-check-inputs.md) before proceeding.

Choose **the same repeatably identifiable reference point** on the workpiece for all measurements. Using a different corner or a different workpiece at the first and second positions will introduce calculation errors. Check the measuring tool/TCP, and ensure that the workpiece will not slip on the conveyor.

{% hint style="warning" %}
Move the tool safely clear before moving the conveyor, and stop the conveyor before moving the robot to the reference point. Do not move the conveyor while the tool is in contact with the workpiece reference point. First ensure sufficient safe space and robot reach. If this cannot be ensured, do not arbitrarily use a short measurement distance and treat the result as acceptable; consult the personnel responsible about an alternative measurement method.
{% endhint %}

### 1. Calculate the Conveyor Travel Direction (Angle)

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

### 2. Calculate the Encoder Resolution

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

### 3. Compare Against Actual Travel

1. Place the tool in a safe position and, with the robot program stopped, open `[Monitoring] > [Sensor Synchronization]`.
2. Use a workpiece detected by the actual limit switch. With the conveyor stopped, record the **Workpiece Position** and the actual location of the reference point.
3. Move the conveyor under approved conditions and stop it. Compare the actual travel distance between the two locations with the change in workpiece position on the monitor. Do not measure during movement.
4. Verify that the change in position corresponds correctly to the travel direction and distance. Compare the converted **Workpiece Position (mm)**, not the raw pulses.
5. Record whether the result meets the predefined distance error criteria. Before and after verifying the resolution, also check that the **Travel Speed (mm/s)** matches the actual test conditions.

### Completion Criteria

- The calculated angle and resolution have been saved for the sensor to be used.
- The actual travel direction corresponds to the position change on the monitor.
- The actual travel distance and the change in workpiece position agree within the predefined tolerance.

### Symptoms and Checks

{% hint style="warning" %}
Do not arbitrarily change the sign or magnitude of a value before identifying the cause.
{% endhint %}

| Symptom | Checks |
| --- | --- |
| Incorrect direction | Check the sensor selection, `S1/S2` order, counter type, and wiring |
| Incorrect distance ratio | Check the linear/circular conveyor selection, pulse/m units, reference point, tool/TCP, and encoder input |
| The value continues to change even when stopped | Check for duplicate signal assignments, board settings, wiring, and noise |
