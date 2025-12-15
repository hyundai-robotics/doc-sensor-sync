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