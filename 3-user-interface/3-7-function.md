# 3.7 Functions

- **cv.position(<workpiece_index>)**

    Use `cv.position` to obtain the current position of a workpiece.

    ### Description
    When multiple workpieces sequentially pass the limit switch, use this to get the distance moved (mm) from the limit switch for each workpiece. Index 0 corresponds to the first-entered workpiece. Index numbers increase in order of entry.

    ### Syntax
    ```python
    result = cv.position(<workpiece_index>)
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
        <td style="text-align:left">Workpiece Index</td>
        <td style="text-align:left">
            Matched sequentially starting from 0 according to the order in which workpieces enter
        </td>
        <td style="text-align:left">Variable</td>
        </tr>
    </tbody>
    </table>


    ### Example
    ```python
    if cv.position(0) > 1000 then
        print "Workpiece has left the allowable work area."
    endif
    ```