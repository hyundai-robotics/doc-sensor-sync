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