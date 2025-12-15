# 5. Frequently Asked Questions

- **If an additional axis has base specifications and the axis configuration is linear, how does conveyor sync operate?**

    When an auxiliary axis exists during conveyor sync, the robot first follows the workpiece using the additional axis. If the robot cannot follow with the auxiliary axis due to soft limits or arm interference, it uses the robot's 6 axes to follow the workpiece.

- **What happens if the B-axis angle passes near 0 degrees during conveyor sync?**

    If the B-axis passes near 0 degrees during conveyor sync, the robot cannot keep the tool orientation stable. When mounting the tool, choose a tool orientation that avoids B-axis angles near 0 degrees.

- **How can I manually input the limit switch?**

    Use the **[Limit Switch Operation]** button in Sensor Sync Monitoring.

- **How can I manually clear current conveyor (press) data?**

    Use the [Manual Reset] button in Sensor Sync Monitoring.
