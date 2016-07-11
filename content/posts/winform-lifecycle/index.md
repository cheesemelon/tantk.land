+++
date = "2016-07-11T17:07:39+09:00"
title = "Winform 이벤트 라이프 사이클"
subtitle = ""
tags = []
draft = true
+++


레지스트리를 가져오고 기록하는 부분은 프로그램이 실행되고 종료될 때가 가장 적절할 것인데, 어느 이벤트에 구현해야 할까?

[MSDN](https://msdn.microsoft.com/ko-kr/library/86faxx0d(v=vs.110).aspx)을 보면 Winform의 한 라이프 사이클에서 여러가지 이벤트가 처리되는데, 주목할 만한 이벤트들을 꼽자면 다음과 같다.

* 시작할 때의 순서
    1. **Form.Load**: Form의 모든 Control들이 생성 완료됐을 때 발생한다.
    2. **Form.Activated**: Form이 활성화 되었을 때 발생한다. 즉, 여러번 발생할 수 있다.
    3. **Form.Shown**: Form의 모든 Control들이 그리기를 완료했을 때 발생한다. Form이 화면에 처음 보여졌을 때 한 번만 호출된다.
* 종료할 때의 순서
    1. **Form.Closing**
    2. Form.FormClosing
    3. Form.Closed
    4. Form.FormClosed
    5. Form.Deactivate 

먼저 Form이 시작될 때를 보자. `Form.Activated`는 여러번 발생할 수 있으므로 후보에서 제외하고, `Form.Load` 혹은 `Form.Shown`을 

# References
* http://www.simpleisbest.net/archive/2006/12/08/1424.aspx