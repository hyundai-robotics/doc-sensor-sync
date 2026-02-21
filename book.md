
[__SOURCE](README.md)
# ${cont_model} 控制器功能手册 - 传感器同步（输送机，压力机）
[__SOURCE](0-about-this-manual/precautions.md)
# 注意事项

{% include url="https://hrcontentsrelay-bmgae5hdbzapc4bc.koreacentral-01.azurewebsites.net/api/proxy?path=doc-common-pages/zh/precautions.md" %}
[__SOURCE](1-intro/README.md)
# 1. 概述

传感器同步是一个基于外部传感器信号执行同步的功能。外部传感器支持编码器，并根据输送机和压力机操作进行分类。

- 输送机同步

    机器人跟踪输送机，并在沿其移动的工件上操作。

- 压力机同步

    通过将压力机的行程与机器人的位置进行同步来执行操作。
[__SOURCE](1-intro/1-1-system-config.md)
# 1.1 系统配置

典型的传送带同步系统配置如下所示。

![](../_assets/image9.png)

- **限位开关**

    一种设备，用于通知控制器工件是否已进入传送带上的特定位置，以及压力机是否已通过特定位置。限位开关的位置作为位置判断的参考点。

- **编码器**

    附加在电动机驱动上的编码器生成与电动机旋转相对应的脉冲。编码器连接到机器人控制器，编码器输出的脉冲输入到机器人控制器。
[__SOURCE](1-intro/1-2-conveyor-sync-principle.md)
# 1.2 输送带同步原理

- **教学**

    例如，考虑在输送带停止时教授点 P1~P7，如下所示。

![](../_assets/image10-1.png)

- **回放**

    如果将 P2~P6 设置为输送带同步区间并回放所教授的轨迹，机器人必须与变化的输送带速度同步，并保持工件与工具之间的相对位置和方向。
    在同步区间内，工件从所教授的参考位置移动的距离为工件通过限位开关后移动的距离，如下所示。

![](../_assets/image10-2.png)
[__SOURCE](1-intro/1-3-press-sync-principle.md)
# 1.3 压力同步原理

压力机从上死点移动到下死点进行压力操作，然后再上升回到顶部，形成一个循环。压力同步记录压力机位置和机器人位置的步骤数据，以便根据压力机的运动速度同步机器人位置。同步性能受到机器人加速/减速和最大速度的限制；与预期压力速度的大幅度偏差可能会导致错误。

![](../_assets/image11.png)
[__SOURCE](1-intro/1-4-major-spec.md)
# 1.4 主要规格

| **项目** | **规格** |
| :------: | :---------------: |
| 可同步传感器数量（输送带，压力机） | 2 |
| 输送带（压力机）形式 | 线性，圆形 |
| 输送带角度设置 | 支持自动设置 |
| 脉冲输入类型 | 开漏，线路驱动 |
| 脉冲计数方法 | 上/下 |
| 编码器分辨率设置 | 支持自动设置 |
| 每个输送带允许的最大工件数量 | 100 |
| 可同步传感器行程 | 21 m |
| 输送带同步部分的插值方法 | 线性 (L)，圆形 (C) |
| 压力机同步部分的插值方法 | 轴插值 (P)，线性 (L)，圆形 (C) |
[__SOURCE](1-intro/1-5-operation-sequence.md)
# 1.5 操作顺序

![](../_assets/image12.png)
[__SOURCE](2-system-config-access/README.md)
# 2. 系统配置和连接

使用输送机同步所需的系统配置如下。

![](../_assets/image15.png)
[__SOURCE](2-system-config-access/2-1-conveyor-if-board.md)
# 2.1 输送机 I/F 板

我们公司支持的输送机 I/F 板如下。有关详细信息，请参阅单独的文档。

- **M5112**

    使用结合网络适配器和电源模块的 Crevis FnIO 模块。

    * 手册：
      https://www.crevis.ru/files/spec/g/[Spec]%20M5112%20(Rev%201.03).pdf
[__SOURCE](2-system-config-access/2-2-hardware-inspection.md)
# 2.2 硬件检查

选择 **[监控 > 传感器同步]** 以检查与传感器同步相关的数据。

![](../_assets/image21.png)

- **限位开关**

    “限位开关输入”字段在限位开关处于活动状态时显示 1，处于非活动状态时显示 0。如果未正常工作，请检查硬件。

- **编码器**

    “原始脉冲”字段显示编码器脉冲；在输送机移动期间，值应在 0~ffff 范围内持续增加或减少。如果未正常工作，请检查硬件。
[__SOURCE](3-user-interface/README.md)
# 3. 用户界面
[__SOURCE](3-user-interface/3-1-conveyor-angle-auto-set/README.md)
# 3.1 输送带角度自动设置

如果输送带方向随意放置，准确测量输送带在3D空间中的运动可能需要相当长的时间。因此，机器人控制器必须事先了解输送带在机器人坐标系中的运动方向以实现机器人同步。  
为此，请使用控制器内置的自动角度计算功能。
[__SOURCE](3-user-interface/3-1-conveyor-angle-auto-set/1-program-teaching.md)
# 3.1.1 程序教学

要执行输送带角度自动计算，首先按如下编写程序。

{% hint style="info" %}  
为了准确设置角度，请尽可能使每个记录位置相距较远（建议直线输送带至少1米）。
{% endhint %}


1. 选择一个新的程序进行输送带角度自动计算。
2. 将机器人工具尖端移动到输送带工件的特定位置并记录S1。

![](../../_assets/image22.png)

3. 移动输送带以移动工件，将机器人工具尖端移动到相同的特定位置并记录S2。

![](../../_assets/image23.png)

4. 将创建一个类似于以下的程序。

![](../../_assets/image24.png)


{% hint style="info" %}    
对于圆形输送带，需要三个位置来计算角度。再重复一次第3步。  
{% endhint %}
[__SOURCE](3-user-interface/3-1-conveyor-angle-auto-set/2-auto-calculation.md)
# 3.1.2 运行自动计算

在 [Settings > Application Parameters > Sensor Sync] 屏幕中，按 [Angle Setting] 显示角度屏幕。

{% hint style="info" %}  
如果传送带类型设置为圆形，则以下设置将被修改以定义圆形传送带的角度和中心。
{% endhint %}

![](../../_assets/image25.png)

1. 检查或手动设置当前配置的传送带角度。
2. 要进行自动计算，请按 [Auto Calculate]，输入教学程序号，计算结果将显示出来。

![](../../_assets/image26.png)

3. 按 [OK] 保存配置的值。
[__SOURCE](3-user-interface/3-2-encoder-resolution-auto-set.md)
# 3.2 编码器分辨率自动设置

编码器分辨率是指线性输送机（或压力机）移动 1 米或圆形输送机（或压力机）旋转 1 度时生成的脉冲数量。

要自动计算编码器分辨率，请转到 [Settings > Application Parameters > Sensor Sync] 并按 [Resolution Calculation]。

![](../_assets/image27.png)

1. 在工件触发限位开关后，如下所示指定传感器。

![](../_assets/image28.png)

2. 选择位置 <1>。
3. 将机器人工具尖端移动到工件上的特定位置。
4. 按 [Record Pose] 记录当前机器人的姿态和编码器脉冲值。
5. 通过按如下所示操作传感器移动工件（至少 1 米）。

![](../_assets/image29.png)

6. 选择位置 <2>。
7. 将机器人工具尖端移动回步骤 3 中记录的特定位置。
8. 按 [Record Pose] 记录姿态和编码器脉冲值。
9. 按 [Calculate Resolution] 计算编码器分辨率并将其记录在编码器分辨率字段中。

重复步骤 1~9 以计算最多四个编码器分辨率。

10. 按 [Calculate Average] 计算记录的编码器分辨率的平均值。
11. 按 [OK] 将平均值设置为编码器分辨率。
[__SOURCE](3-user-interface/3-3-sensor-sync-parameter.md)
# 3.3 传感器同步参数

为了应用输送带（压机）同步并执行机器人运动，机器人控制器必须了解与之同步的输送带（压机）的各种信息。这些参数必须在编写操作程序之前设置。

![](../_assets/image30.png)

- **输送带形式**

    根据下图选择形式。

![](../_assets/image31.png)

- **编码器分辨率**

    编码器分辨率定义为线性输送带移动1米或圆形输送带旋转1度时生成的脉冲数量。

{% hint style="info" %}
有关自动编码器分辨率计算，请参见[3.2 编码器分辨率自动设置](3-2-encoder-resolution-auto-set.md)部分。
{% endhint %}

- **输送带允许速度**

    此参数用于将异常高的速度视为错误。根据预期的操作速度进行配置；控制器内部计算输送带速度，并在速度超过配置的允许速度时报告错误。

{% hint style="info" %}  
编码器脉冲通常在其平均值周围有波动，因此速度也会显示轻微的波动。将允许速度设置得稍高一些以适应这种情况。
{% endhint %}

- **脉冲异常检测的允许计数**

    如果脉冲输入异常，机器人控制器会输出错误“E0019 输送带脉冲允许频率超出”。配置此项以允许机器人通过容忍在同步操作期间的某些脉冲异常而继续工作（保护工件）。

{% hint style="info" %}  
例如，如果允许的脉冲异常检测个数设置为3，则即使在同步操作期间对单个工件检测到三次脉冲异常，机器人控制器也不会生成错误，而是在内部生成适当的脉冲值。当检测到第四个脉冲异常时，会生成错误。针对该工件的播放完成后，检测到的脉冲异常数量信息将被重置。
{% endhint %}  

- **允许的最大工件数量**

    配置当另一个工件触发极限开关并在机器人同步一个工件时进入工作空间时，机器人是否应继续工作。最大允许100个。

- **检测同步相关系统错误**

    如果系统安装不完整或电路板损坏导致无法切换到运行准备状态的同步相关系统错误，请配置以忽略同步相关系统错误，以便继续进行无关操作。

| **错误编号** | **同步系统错误类型** |
| :-----------: | ------------------------- |
| E0021 | 超过输送带允许速度 |

- **同步复位输入**
外部输入可以清除输送机（按下）数据。当机器人停止时输入此信号时，传感器相关数据（脉冲数据、工件位置、速度、工件数量、同步播放状态等）将被清除，相当于手动重置。

- **限位开关输入**

    通过外部输入接受限位开关状态。

- **脉冲计数器输入**

    通过外部输入接受编码器脉冲计数器。控制器内部将脉冲计数器管理为16位数据，因此使用1字（2字节）输入信号。指定最低位信号编号会自动分配16个连续信号。

![](../_assets/image32.png)

- **脉冲线错误**

    使用线路驱动脉冲通信时，可以检测脉冲线路上的开路。使用此功能向机器人控制器报告脉冲线路错误。

- **输送机同步开启**

    输送机同步状态可以外部输出。当命令**"cv.sync start"**运行并且同步开启时，输出**"1"**。

- **脉冲计数器类型**

    当输送机向前移动时，脉冲值增加。如果在向后移动时脉冲增加，则为“上”类型；如果减少，则使用“上/下”类型。通常使用“上/下”。在我们的板上，“上”输出为关闭，而“上/下”则为开启。

- **脉冲通信类型**

    典型的脉冲通信使用开集电极和线路驱动。有关详细信息，请参考单独的学习材料。在我们的板上，“线路驱动”关闭，而“开集电极”开启。
[__SOURCE](3-user-interface/3-4-monitoring.md)
# 3.4 监控

选择 **[监控 > 传感器同步]** 查看与传感器同步相关的数据。使用 **[传感器同步. 操作]** 按钮执行各种操作。

![](../_assets/image33.png)

- **脉冲数据**

    自限位开关以来计数的工件脉冲数。

- **工件位置**

    工件从限位开关移动的距离。对于 <linear> 形式，以毫米为单位；对于 <circular> 形式，以度为单位。

- **速度**

    输送带（或压机）的移动速度。对于 <linear> 形式，以毫米/秒为单位；对于 <circular> 形式，以度/秒为单位。

- **已输入的工件数量**

    触发限位开关并进入的工件数量。

- **限位开关输入**

    显示限位开关是否处于活动状态。

- **原始脉冲**

    在正常操作期间，以十六进制数据（0~ffff）显示编码器脉冲计数器值。

- **手动重置**

    手动清除传感器相关数据（脉冲数据、工件位置、速度、工件数量、同步播放状态等）。

- **输入工件位置**

    手动输入传感器位置值（线性以毫米为单位，圆形以度为单位）。

- **限位开关操作**

    当需要手动切换限位开关时使用。
[__SOURCE](3-user-interface/3-5-step-data.md)
# 3.5 步骤数据

当传感器同步设置为 <enabled> 且您按下 [Record] 时，当前机器人的轴位置以及当前工件位置将被记录，如下所示。

机器人在输送带同步播放期间使用记录的位置数据。

![](../_assets/image34.png)

您可以在当前步骤位置属性的 ss# 字段中查看和编辑录制的工件位置。

![](../_assets/image35.png)
[__SOURCE](3-user-interface/3-6-command.md)
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
[__SOURCE](3-user-interface/3-7-function.md)
# 3.7 功能

- **cv.position(<workpiece_index>)**

    使用 `cv.position` 获取工件的当前位置。

    ### 描述
    当多个工件依次通过限位开关时，使用此方法获取每个工件从限位开关移动的距离（毫米）。索引 0 对应于第一个进入的工件。索引号按照进入顺序递增。

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
            从 0 开始按工件进入的顺序依次匹配
        </td>
        <td style="text-align:left">变量</td>
        </tr>
    </tbody>
    </table>


    ### 示例
    ```python
    if cv.position(0) > 1000 then
        print "工件已离开可允许的工作区域。"
    endif
    ```
[__SOURCE](3-user-interface/3-8-variable.md)
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

- **cv.work_no (输入工件数量)**

    ### 描述
    用于读取通过限位开关后进入输送机的工件数量；对应于监控输入工件计数。

    ### 示例
    ```python
    if cv.work_no > 30 then
        print "超过允许的输入工件数量。"
    endif
    ```

- **cv.raw_pulse (编码器原始脉冲)**

    ### 描述
    用于读取来自编码器的当前脉冲计数器输入；对应于监控原始脉冲。
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
[__SOURCE](4-teaching/README.md)
# 4. 教学

为传感器同步编写程序遵循相同的一般教学工作流程。但是，要执行传感器同步回放，您必须使用命令 **cv.sync (sync playback)** 和 **cv.wait (sensor interlock wait)**；这些命令必须在回放之前记录在教学程序中。
[__SOURCE](4-teaching/4-1-sync-oper-program-config.md)
# 4.1 同步操作程序结构

- **回原点等待**

    机器人在其原点位置等待，直到输入启动命令。

- **联锁等待**

    机器人移动到接近同步区，并等待工件到达 `cv.wait` 记录的距离。

下图显示了在输送机上流动的工件的喷涂程序。机器人在向步骤 S4 前进时开始输送机同步，并从步骤 S5 开始同步喷涂。联锁等待步骤 (S3) 记录在同步区入口步骤 (S4) 附近。

![](../_assets/image_1.png)

示例程序：

```python
    global cv
    cv = sync.Sensor(1)
    cv.sync reset               # 输送机同步重置
S1                              # 机器人回原点
S2
S3                             # 联锁等待步骤
    cv.sync on                 # 启动输送机同步
    cv.wait posi=500,sync=0    # 输送机联锁等待
S4                             # 同步区入口步骤
    do1 = 1                    # 喷涂信号开启
S5                             # 第一步同步操作步骤
 :
S9                             # 最后一步同步操作步骤
    do1 = 0                    # 喷涂信号关闭
    cv.sync off                # 结束输送机同步
                                # 完成当前工作
S10
 :
S13                            # 机器人回原点
    end
```

- **同步播放**

    在图中，输送机同步播放部分指的是步骤 S4 到 S9；这一部分的所有命令都与移动的输送机同步执行。

- **返回原点**

    完成操作后，机器人返回到原点位置，以等待下一个启动命令。
[__SOURCE](4-teaching/4-2-press-sync-teaching.md)
# 4.2 按压同步教学

按压同步使机器人跟随按压速度。假设按压速度是恒定的；如果按压速度变化，同步性能会下降。在 **"允许速度"** 下的传感器同步参数设置中设置当前允许的按压速度。

使用按压同步的示例程序：

```python
    global press
    press = sync.Sensor(1)
    press.sync reset              # 按压同步重置
S1
    press.sync on                 # 开始按压同步
    press.wait posi=500,sync=0    # 按压联锁等待
S2  move P,spd=60%                # 记录传感器1的位置
S3  move P,spd=60%                # 记录传感器1的位置
S4  move P,spd=60%                # 记录传感器1的位置
    press.sync off                # 结束按压同步
S5
    end
```

在上述程序中，第二、第三和第四步的传感器记录位置必须严格递增；否则会发生以下错误：

| **错误代码** | **错误消息** |
| :----------: | ------------- |
| E0239       | 步骤传感器位置未严格递增。 |

此外，步骤2、3和4记录的速度被忽略；运动是基于用户配置的允许按压速度进行规划的。如果记录的传感器和机器人位置要求的运动超过机器人能力，即使在最大速度下规划，操作过程中会发生以下错误：

| **错误代码** | **错误消息** |
| :----------: | ------------- |
| E0238       | 无法跟随传感器速度。 |
[__SOURCE](5-faq.md)
# 5. 常见问题解答

- **如果附加轴具有基本规格且轴配置为线性，输送带同步是如何工作的？**

    当在输送带同步期间存在辅助轴时，机器人首先通过附加轴跟随工件。如果由于软限制或手臂干涉导致机器人无法用辅助轴跟随，则使用机器人的6个轴跟随工件。

- **如果B轴角度在输送带同步期间接近0度，会发生什么？**

    如果B轴在输送带同步期间接近0度，机器人无法保持工具方向稳定。在安装工具时，选择避免B轴角度接近0度的工具方向。

- **如何手动输入限位开关？**

    在传感器同步监控中使用**[限位开关操作]**按钮。

- **如何手动清除当前输送带（按下）数据？**

    在传感器同步监控中使用[手动重置]按钮。