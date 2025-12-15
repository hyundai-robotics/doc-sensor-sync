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
