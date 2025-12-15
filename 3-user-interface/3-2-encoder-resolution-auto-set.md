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
