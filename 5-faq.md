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
