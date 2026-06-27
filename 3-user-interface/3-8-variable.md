```markdown
# 3.8 变量

- **cv.speed (输送机速度)**

    ### 描述
    用于读取输送机或压力机的运动速度；对应于监控速度。

    ### 示例
    ```python
    if cv.speed > 300 then
        print "输送机速度过高。"
    endif
    ```

- **cv.pulse (工件脉冲)**

    ### 描述
    用于读取工件从限位开关移动的脉冲（距离）；对应于监控脉冲数据。

    ### 示例
    ```python
    if cv.pulse > 10000 then
        print "工件已离开允许的工作区域。"
    endif
    ```

- **cv.position (工件位置)**
    ### 示例
    ```python
    if cv.position > 1000 then
        print "工件已离开允许的工作区域。"
    endif
    ```

- **cv.work_no (已输入工件数量)**

    ### 描述
    用于读取经过限位开关后进入输送机的工件数量；对应于监控已输入工件计数。

    ### 示例
    ```python
    if cv.work_no > 30 then
        print "超过允许输入工件的数量。"
    endif
    ```

- **cv.raw_pulse (编码器原始脉冲)**

    ### 描述
    用于读取来自编码器的当前脉冲计数输入；对应于监控原始脉冲。

    ### 示例
    ```python
    var raw_pulse = cv.raw_pulse
    ```

- **cv.resolution (编码器分辨率)**

    ### 描述
    用于读取用户设置的编码器分辨率。

    ### 示例
    ```python
    var resolution = cv.resolution
    ```
```