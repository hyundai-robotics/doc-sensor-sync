# 3.7 함수

*   <mark style="color:green;">**cv.position(<작업물 인덱스>)**</mark> <mark style="color:blue;"></mark>

    cv.position 함수를 사용하면 작업물의 현재 위치를 얻을 수 있습니다.

    ### 설명
    복수의 작업물이 리밋스위치를 순차적으로 통과한 경우, 각각의 작업물에 대해서 리밋스위치를 기준으로 이동한 거리(mm)를 얻고자 할 때 사용합니다. 작업물 인덱스로 0 이 가장 먼저 진입한 작업물이며, 이는 cv.position 과 동일합니다. 이를 기준으로 작업물 인덱스 번호는 진입한 순서에 따라 증가하여 매칭됩니다.

    ### 문법

    ```python
    result=cv.position(<작업물 인덱스>)
    ```
    
    ### 파라미터
    <table>
    <thead>
        <tr>
        <th style="text-align:left">항목</th>
        <th style="text-align:left">의미</th>
        <th style="text-align:left">기타</th>
        </tr>
    </thead>
    <tbody>
        <tr>
        <td style="text-align:left">작업물 인덱스</td>
        <td style="text-align:left">
            진입한 작업물 순서에 따라 0 부터 순차적으로 증가하여 매칭
        </td>
        <td style="text-align:left">변수</td>
        </tr>
    </tbody>
    </table>


    ### 사용 예

    ```python
    if cv.position(0)>1000 then
        print "작업물이 허용 작업 영역을 벗어났습니다."
    endif
    ```