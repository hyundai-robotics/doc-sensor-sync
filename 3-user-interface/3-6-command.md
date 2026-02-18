# 3.6 命令

- **cv.sync (同步播放)**

    ### 描述:

    指定在程序播放期间执行传感器同步的部分。

    ### 语法:

    ```python
    cv.sync <sync_action>
    ```

    ### 参数
    <table>
    <thead>
        <tr>
            <th style="text-align:left">项目</th>
            <th style="text-align:left">描述</th>
            <th style="text-align:left">备注</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td style="text-align:left">同步操作</td>
            <td style="text-align:left">
                - reset: 重置同步。 (同步关闭 + 清除传送带数据)<br>
                - on: 开始同步。 (同步开启)<br>
                - off: 暂停同步。 (同步关闭)<br>
                - next: 为下一个工件开始同步 (同步关闭 + 加载下一个工件数据)<br>
            </td>
            <td style="text-align:left">字符串</td>
        </tr>
    </tbody>
    </table>


- **cv.wait (联锁等待)**

    ### 描述:

    用于暂停机器人，直到工件从限位开关到达指定位置。

    ### 语法:
    ```python
    cv.wait posi=<wait_distance>,sync=<sync_flag>
    ```

    ### 参数:
- wait_distance: 从限位开关到等待工件的距离（可变）
    - sync_flag: 0 = 异步，1 = 同步（不支持压力机）

- **cv.input（工件进入）**

    ### 描述：

    当限位开关触发时使用，以注册一个工件已进入。

    ### 语法：
    ```
    cv.input
    ```

### 示例
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