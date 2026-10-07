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