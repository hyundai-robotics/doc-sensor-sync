# 3.8 변수

*   <mark style="color:green;">**cv.speed (컨베이어 속도)**</mark>

    컨베이어나 프레스의 이동 속도를 읽을 때 사용되며 모니터링 상의 이동속도에 해당하는 값입니다.

```
if cv.speed>300 then
    print "컨베이어 속도가 너무 큽니다."
endif
```

*   <mark style="color:green;">**cv.pulse (작업물 펄스)**</mark>

    동기 작업을 위해 작업물이 리밋스위치로부터 이동한 펄스(pulse)를 인식하고자 할 때 사용되며 모니터링 상의 펄스 데이터에 해당하는 값입니다.

```
if cv,pulse>10000 then
    print "작업물이 허용 작업 영역을 벗어났습니다."
endif
```

* <mark style="color:green;">**cv.position (작업물 위치)**</mark>

```
if cv.position>1000 then
    print "작업물이 허용 작업 영역을 벗어났습니다."
endif
```

*   <mark style="color:green;">**cv.work\_no (진입한 작업물 개수)**</mark>

    리밋스위치를 통과하여 컨베이어상에 진입한 작업물의 개수를 읽을 때 사용되며 모니터링 상의 진입 작업물 개수에 해당하는 값입니다.

```
if cv.work_no>30 then
    print "진입 작업물 개수를 초과하습니다."
endif
```

*   <mark style="color:green;">**cv.raw\_pulse (엔코더 raw pulse)**</mark>

    엔코더로부터 입력된 현재의 pulse 카운터를 읽을 때 사용되며 모니터링 상의 raw pulse 에 해당하는 값입니다.

```
var raw_pulse=cv.raw_pulse
```

*   <mark style="color:green;">**cv.resolution (엔코더 분해능)**</mark>

    사용자가 설정한 엔코더 분해능을 읽을 때 사용합니다.

```
var resolution=cv.resolution
```
