
[__SOURCE](README.md)
# ${cont_model} 控制器功能手册 - 传感器同步（输送机，压力机）
[__SOURCE](0-about-this-manual/README.md)
# 关于手册
[__SOURCE](0-about-this-manual/precautions.md)
# 注意事项

{% include file="zh/precautions.md" %}
[__SOURCE](0-about-this-manual/safety-notice.md)
# 安全注意事项

{% include file="zh/safety-notice.md" %}
[__SOURCE](1-intro/README.md)
# 1. 概述

传感器同步是一种基于外部传感器信号执行同步的功能。外部传感器支持编码器，并根据传送带和压机操作进行分类。

- 传送带同步

    机器人追踪传送带，并在沿其移动的工件上操作。

- 压机同步

    操作通过将压机的行程与机器人的位置同步来执行。
[__SOURCE](1-intro/1-1-system-config.md)
# 1.1 系统配置

传送带同步系统的典型配置如下所示。

![](../_assets/image9.png)

- **限位开关**

    一种设备，用于通知控制器工件是否已进入传送带上的特定位置，以及压力机是否已通过特定位置。限位开关的位置作为位置判断的参考点。

- **编码器**

    附加在电机驱动上的编码器产生与电机旋转相对应的脉冲。编码器连接到机器人控制器，来自编码器的脉冲输出输入到机器人控制器。
[__SOURCE](1-intro/1-2-conveyor-sync-principle.md)
# 1.2 输送带同步原理

- **教学**

    例如，在输送带停止时考虑教导点 P1~P7，如下所示。

![](../_assets/image10-1.png)

- **回放**

    如果将 P2~P6 设置为输送带同步段，并回放所教的轨迹，机器人必须与变化的输送带速度同步，并保持工件与工具之间的相对位置和方向。
    在同步段内，工件从教导的参考位置移位，移位距离为工件经过限位开关后移动的距离，如下所示。

![](../_assets/image10-2.png)
[__SOURCE](1-intro/1-3-press-sync-principle.md)
# 1.3 压力同步原理

压力从上止点移动到下止点以执行压力操作，然后再次上升到顶部，形成一个周期。压力同步记录压力位置和机器人位置的步进数据，以根据压力运动速度同步机器人位置。同步性能受到机器人的加速/减速和最大速度的限制；与预期压力速度的较大偏差可能会导致错误。

![](../_assets/image11.png)
[__SOURCE](1-intro/1-4-major-spec.md)
# 1.4 主要规格

| **项目** | **规格** |
| :------: | :---------------: |
| 可同步传感器数量（输送机，压力机） | 2 |
| 输送机（压力机）形式 | 线性，圆形 |
| 输送机角度设置 | 支持自动设置 |
| 脉冲输入类型 | 开放集电极，线路驱动 |
| 脉冲计数方法 | 上/下 |
| 编码器分辨率设置 | 支持自动设置 |
| 每个输送机允许的最大工件数量 | 100 |
| 可同步传感器行程距离 | 21 m |
| 输送机同步部分的插值方法 | 线性（L），圆形（C） |
| 压力机同步部分的插值方法 | 轴插值（P），线性（L），圆形（C） |
[__SOURCE](1-intro/1-5-operation-sequence.md)
# 1.5 操作顺序

![](../_assets/image12.png)
[__SOURCE](2-system-config-access/README.md)
# 2. 系统配置和连接

使用输送机同步所需的系统配置如下。

![](../_assets/image15.png)
[__SOURCE](2-system-config-access/2-1-conveyor-if-board.md)
# 2.1 输送机 I/F 板

我们公司支持的输送机 I/F 板如下。详情请参考单独的文档。

- **M5112**

    使用与网络适配器和电源模块结合的 Crevis FnIO 模块。

    * 手册：
      https://www.crevis.ru/files/spec/g/[Spec]%20M5112%20(Rev%201.03).pdf
[__SOURCE](2-system-config-access/2-2-hardware-inspection.md)
# 2.2 硬件检查

选择 **[Monitoring > Sensor Sync]** 检查与传感器同步相关的数据。

![](../_assets/image21.png)

- **限位开关**

    "限位开关输入"字段在限位开关活动时显示1，不活动时显示0。如果没有正常操作，请检查硬件。

- **编码器**

    "原始脉冲"字段显示编码器脉冲；在输送机移动期间，该值应在范围0~ffff内持续增加或减少。如果没有正常操作，请检查硬件。
[__SOURCE](3-user-interface/README.md)
# 3. 用户界面
[__SOURCE](3-user-interface/3-1-conveyor-angle-auto-set/README.md)
# 3.1 输送带角度自动设置

如果输送带方向任意放置，准确测量输送带在三维空间中的移动可能需要相当长的时间。因此，机器人控制器必须事先知道输送带在机器人坐标系中的运动方向，以便进行机器人同步。  
为此，请使用控制器内置的自动角度计算功能。
[__SOURCE](3-user-interface/3-1-conveyor-angle-auto-set/1-program-teaching.md)
# 3.1.1 程序教学

要执行输送机角度自动计算，首先编写如下程序。

{% hint style="info" %}  
为了准确设置角度，请尽可能使每个记录的位置相距较远（建议直线输送机至少为 1 米）。
{% endhint %}


1. 选择一个新的程序用于输送机角度自动计算。
2. 将机器人工具尖端移动到输送机工件的特定位置并记录 S1。

![](../../_assets/image22.png)

3. 移动输送机以移动工件，将机器人工具尖端移动到同一特定位置并记录 S2。

![](../../_assets/image23.png)

4. 将创建一个类似于以下内容的程序。

![](../../_assets/image24.png)


{% hint style="info" %}    
对于圆形输送机，计算角度需要三个位置。再重复一次步骤 3。  
{% endhint %}
[__SOURCE](3-user-interface/3-1-conveyor-angle-auto-set/2-auto-calculation.md)
# 3.1.2 自动计算运行

在[设置 > 应用程序参数 > 传感器同步]屏幕中，按[角度设置]以显示角度屏幕。

{% hint style="info" %}  
如果输送机类型设置为圆形，则下面的设置将被修改，以定义圆形输送机的角度和中心。
{% endhint %}

![](../../_assets/image25.png)

1. 检查或手动设置当前配置的输送机角度。
2. 要进行自动计算，请按[自动计算]，输入教学程序编号，计算结果将显示出来。

![](../../_assets/image26.png)

3. 按[确定]以保存配置的值。
[__SOURCE](3-user-interface/3-2-encoder-resolution-auto-set.md)
# 3.2 编码器分辨率自动设置

编码器分辨率是指当线性输送带（或压机）移动 1 m 时，或当圆形输送带（或压机）旋转 1 度时生成的脉冲数。

要自动计算编码器分辨率，请转到 [Settings > Application Parameters > Sensor Sync] 并按 [Resolution Calculation]。

![](../_assets/image27.png)

1. 在工件触发限位开关后，如下所示指定传感器。

![](../_assets/image28.png)

2. 选择位置 <1>。
3. 将机器人工具尖端移动到工件上的特定位置。
4. 按 [Record Pose] 记录当前机器人姿态和编码器脉冲值。
5. 按照所示操作传感器移动工件（至少 1 m）。

![](../_assets/image29.png)

6. 选择位置 <2>。
7. 将机器人工具尖端移回步骤 3 中记录的特定位置。
8. 按 [Record Pose] 记录姿态和编码器脉冲值。
9. 按 [Calculate Resolution] 计算编码器分辨率并将其记录在编码器分辨率字段中。

重复步骤 1~9 以计算最多四个编码器分辨率。

10. 按 [Calculate Average] 计算记录的编码器分辨率的平均值。
11. 按 [OK] 将平均值设为编码器分辨率。
[__SOURCE](3-user-interface/3-3-sensor-sync-parameter.md)
# 3.3 传感器同步参数

为了应用输送机（压力机）同步和执行机器人动作，机器人控制器必须了解与之同步的输送机（压力机）的各种信息。这些参数必须在编写操作程序之前设置。

![](../_assets/image30.png)

- **输送机形式**

    根据下图选择形式。

![](../_assets/image31.png)

- **编码器分辨率**

    编码器分辨率定义为线性输送机移动 1 米或圆形输送机旋转 1 度时产生的脉冲数量。

{% hint style="info" %}
有关自动编码器分辨率计算，请参阅[3.2 编码器分辨率自动设置](3-2-encoder-resolution-auto-set.md)部分。
{% endhint %}

- **输送机允许速度**

    此参数用于将异常高的速度视为错误。根据预期操作速度进行配置；控制器会内部计算输送机速度，并在速度超过配置的允许速度时报告错误。

{% hint style="info" %}  
编码器脉冲通常围绕其平均值有波动，因此速度也会显现出轻微的波动。将允许速度设定得稍高一些以适应这一点。
{% endhint %}

- **脉冲异常检测的允许计数**

    如果脉冲输入异常，机器人控制器会输出错误“E0019 输送机脉冲允许频率超出”。配置此项以允许机器人在同步操作期间容忍一定数量的脉冲异常，从而继续工作（保护工件）。

{% hint style="info" %}  
例如，如果允许的脉冲异常检测次数设置为 3，机器人控制器即使在同步操作期间对单个工件检测到多达三次脉冲异常，也不会产生错误，而是内部生成适当的脉冲值。当检测到第四个脉冲异常时，将生成错误。检测到的脉冲异常次数信息在对应工件的回放完成时重置。
{% endhint %}  

- **允许的最大工件数量**

    配置当另一个工件触发限制开关并在机器人同步一个工件时进入工作空间时，机器人是否应继续工作。允许的最大值为 100。

- **检测与同步相关的系统错误**

    如果系统安装不完整或板损坏，导致阻止切换到 RUN READY 的与同步相关的系统错误，请配置以忽略与同步相关的系统错误，以便可以继续不相关的操作。

| **错误编号** | **同步系统错误类型** |
| :-----------: | ------------------------- |
| E0021 | 输送机允许速度超出 |

- **同步复位输入**

    外部输入可以清除输送机（压力机）数据。当此信号在机器人停止时输入时，传感器相关数据（脉冲数据、工件位置、速度、工件数量、同步回放状态等）被清除，相当于手动复位。

- **限位开关输入**

    通过外部输入接受限位开关状态。

- **脉冲计数器输入**

    通过外部输入接受编码器脉冲计数器。控制器将脉冲计数器内部管理为 16 位数据，因此使用 1 字（2 字节）输入信号。指定最低位信号编号会自动分配 16 个连续信号。

![](../_assets/image32.png)

- **脉冲线错误**

    使用线驱动脉冲通信时，可以检测脉冲线上的开路。使用此功能将脉冲线错误报告给机器人控制器。

- **输送机同步 ON**

    输送机同步状态可以外部输出。当命令 **"cv.sync start"** 运行并且同步处于 ON 时，输出 **"1"**。

- **脉冲计数器类型**

    当输送机向前移动时，脉冲值增加。如果在后退时脉冲增加，则为“上”类型；如果减少，则使用“上下”类型。通常使用“上下”。在我们的板上，“上”被输出为关闭，而“上下”被输出为打开。

- **脉冲通信类型**

    典型的脉冲通信使用开集电极和线驱动。有关详细信息，请参阅单独的学习材料。在我们的板上，“线驱动”被输出为关闭，而“开集电极”被输出为打开。
[__SOURCE](3-user-interface/3-4-monitoring.md)
# 3.4 监控

选择 **[监控 > 传感器同步]** 以查看与传感器同步相关的数据。使用 **[传感器同步. 操作]** 按钮执行各种操作。

![](../_assets/image33.png)

- **脉冲数据**

    自限位开关以来，工件计数的脉冲数量。

- **工件位置**

    工件自限位开关移动的距离。对于 <linear> 形式为 mm；对于 <circular> 形式为度。

- **速度**

    输送机（或压机）的运动速度。对于 <linear> 形式为 mm/s；对于 <circular> 形式为 deg/s。

- **进入的工件数量**

    已触发限位开关并进入的工件数量。

- **限位开关输入**

    显示限位开关是否处于活动状态。

- **原始脉冲**

    在正常操作期间以十六进制数据（0~ffff）显示编码器脉冲计数器值。

- **手动复位**

    手动清除与传感器相关的数据（脉冲数据、工件位置、速度、工件数量、同步播放状态等）。

- **输入工件位置**

    手动输入传感器位置值（线性为 mm，圆形为度）。

- **限位开关操作**

    在需要手动切换限位开关时使用。
[__SOURCE](3-user-interface/3-5-step-data.md)
# 3.5 步骤数据

当传感器同步设置为 <enabled> 并且您按下 [Record] 时，当前机器人轴位置和当前工件位置会被记录，如下所示。

机器人在传送带同步播放期间使用记录的位置信息。

![](../_assets/image34.png)

您可以在当前步骤位置属性的 ss# 字段中查看和编辑记录的工件位置。

![](../_assets/image35.png)
[__SOURCE](3-user-interface/3-6-command.md)
# 3.6 命令

- **cv.sync (同步播放)**

    ### 描述：

    在程序播放期间指定执行传感器同步的部分。

    ### 语法：

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
                - reset: 重置同步。 (同步关闭 + 清除输送带数据)<br>
                - on: 启动同步。 (同步开启)<br>
                - off: 暂停同步。 (同步关闭)<br>
                - next: 为下一个工件启动同步。 (同步关闭 + 加载下一个工件数据)<br>
            </td>
            <td style="text-align:left">字符串</td>
        </tr>
    </tbody>
    </table>


- **cv.wait (互锁等待)**

    ### 描述：

    用于暂停机器人，直到工件到达限位开关的指定位置。

    ### 语法：
    ```python
    cv.wait posi=<wait_distance>,sync=<sync_flag>
    ```

    ### 参数：
    - wait_distance: 等待工件的限位开关距离（变量）
    - sync_flag: 0 = 异步, 1 = 同步（不支持压机）

- **cv.input (工件进入)**

    ### 描述：

    当限位开关触发时，用于注册一个工件已进入。

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
[__SOURCE](3-user-interface/3-7-function.md)
# 3.7 功能

- **cv.position(<workpiece_index>)**

    使用 `cv.position` 获取工件的当前位置信息。

    ### 描述
    当多个工件顺序通过限位开关时，使用此方法获取每个工件从限位开关移动的距离（毫米）。索引 0 对应第一个进入的工件。索引号按进入顺序递增。

    ### 语法
    ```python
    result = cv.position(<workpiece_index>)
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
        <td style="text-align:left">工件索引</td>
        <td style="text-align:left">
            根据工件进入的顺序从0开始按顺序匹配
        </td>
        <td style="text-align:left">变量</td>
        </tr>
    </tbody>
    </table>


    ### 示例
    ```python
    if cv.position(0) > 1000 then
        print "工件已离开允许的工作区域。"
    endif
    ```
[__SOURCE](3-user-interface/3-8-variable.md)
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
[__SOURCE](4-teaching/README.md)
# 4. 教学

编写传感器同步的程序遵循相同的一般教学工作流程。然而，要执行传感器同步播放，您必须使用命令 [cv.sync](../3-user-interface/3-6-command.md)（同步播放）和 [cv.wait](../3-user-interface/3-6-command.md)（传感器互锁等待）；这些命令必须在播放之前录入到教学程序中。
[__SOURCE](4-teaching/4-1-sync-oper-program-config.md)
# 4.1 同步操作程序结构

- **回家位置等待**

    机器人在其回家位置等待，直到输入启动命令。

- **互锁等待**

    机器人移动到同步区域附近并等待工件达到由 `cv.wait` 记录的距离。

下图显示了在传送带上流动的工件的喷涂程序。机器人在前进到步骤 S4 时开始传送带同步，并从步骤 S5 开始同步喷涂油漆。互锁等待步骤（S3）记录在同步区域入口步骤（S4）附近。

![](../_assets/image_1.png)

示例程序：

```python
    global cv
    cv = sync.Sensor(1)
    cv.sync reset               # 传送带同步重置
S1                              # 机器人回家
S2
S3                             # 互锁等待步骤
    cv.sync on                 # 启动传送带同步
    cv.wait posi=500,sync=0    # 传送带互锁等待
S4                             # 同步区域入口步骤
    do1 = 1                    # 喷漆开启信号
S5                             # 第一步同步操作步骤
 :
S9                             # 最后一步同步操作步骤
    do1 = 0                    # 喷漆关闭信号
    cv.sync off                # 结束传送带同步
                                # 完成当前工作
S10
 :
S13                            # 机器人回家
    end
```

- **同步播放**

    在图中，传送带同步播放区域指步骤 S4 到 S9；这一部分的所有命令都与移动的传送带同步执行。

- **返回回家位置**

    完成操作后，机器人返回其回家位置以等待下一个启动命令。
[__SOURCE](4-teaching/4-2-press-sync-teaching.md)
# 4.2 按压同步教学

按压同步使机器人跟随按压速度。假设按压速度是恒定的；如果按压速度变化，同步性能会下降。在**"允许速度"**下的传感器同步参数设置中设置当前允许的按压速度。

使用按压同步的示例程序：

```python
    global press
    press = sync.Sensor(1)
    press.sync reset              # 按压同步重置
S1
    press.sync on                 # 开始按压同步
    press.wait posi=500,sync=0    # 按压互锁等待
S2  move P,spd=60%                # 记录传感器 1 的位置
S3  move P,spd=60%                # 记录传感器 1 的位置
S4  move P,spd=60%                # 记录传感器 1 的位置
    press.sync off                # 结束按压同步
S5
    end
```

在上述程序中，步骤 2、3 和 4 中传感器记录的位置必须严格递增；否则会出现以下错误：

| **错误代码** | **错误信息** |
| :------------: | ----------------- |
| E0239          | 步骤传感器位置未严格递增。 |

此外，步骤 2、3 和 4 中记录的速度将被忽略；运动是基于用户配置的允许按压速度进行规划的。如果记录的传感器和机器人位置要求的运动超过机器人能力，即使在最大速度下规划，运行期间也会出现以下错误：

| **错误代码** | **错误信息** |
| :------------: | ----------------- |
| E0238          | 无法跟随传感器速度。 |
[__SOURCE](5-faq.md)
# 5. 常见问题解答

- **如果附加轴具有基本规格并且轴配置为线性，传送带同步是如何工作的?**

    在传送带同步期间，如果存在辅助轴，机器人首先使用附加轴跟随工件。如果由于软限制或手臂干涉，机器人无法使用辅助轴跟随，则使用机器人的 6 个轴跟随工件。

- **如果 B 轴角度在传送带同步期间接近 0 度会发生什么?**

    如果 B 轴在传送带同步期间接近 0 度，机器人无法保持工具方向稳定。安装工具时，请选择避免 B 轴角度接近 0 度的工具方向。

- **我如何手动输入限位开关?**

    在传感器同步监控中使用 **[限位开关操作]** 按钮。

- **我如何手动清除当前传送带（按）数据?**

    在传感器同步监控中使用 [手动重置] 按钮。