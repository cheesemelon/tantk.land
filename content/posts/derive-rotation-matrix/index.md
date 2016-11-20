+++
date = "2016-11-15T22:45:08+09:00"
title = "회전 행렬(Rotation matrix) 유도"
subtitle = "선형 변환 성질을 이용한 회전 행렬의 유도"
tags = ["CG", "Math"]
+++

회전 행렬이 선형 변환임을 이용해 유도할 수 있다.

(1, 0) t만큼 회전 (cost sint)
(0, 1) t만큼 회전 (-sint cost)

R(x, y) = R(x, 0) + R(0, y)
= x R (1, 0) + y R (0, 1)
= x (cos t, sin t) + y (-sin t, cos t)
= (cos t, -sin t) (x)
  (sin t, cos t)  (y)
