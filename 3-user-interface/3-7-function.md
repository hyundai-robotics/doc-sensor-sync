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