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
