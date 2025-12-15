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
