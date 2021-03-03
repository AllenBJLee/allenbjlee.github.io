---
layout: post
title:  "Vector3 에서 제공되는 이동방법 4가지"
search: true
date: 2021-03-03
category: Unity
---

#### 01. MoveTowards - 목표 지점으로 다이렉트 이동
```c#
transform.position = Vector3.MoveTowards( transform.position ,m_vecTarget, 1f );
```

#### 02. SmoothDamp - 목표 지점으로 스무스하게 이동 (미끄러지듯 감속 이동)
```c#
Vector3 velo = Vector3.zero;
transform.position = Vector3.SmoothDamp( transform.position, m_vecTarget, ref velo, 0.3f );
```

#### 03. Lerp - 선형 보간 이동
```c#
Vector3 vecTarget = new Vector3( 5, 0.25f, 0 );
transform.position = Vector3.Lerp( transform.position, vecTarget, 0.05f );
```

#### 04. Slerp - 선형 포물선 보간 이동
```c#
Vector3 vecTarget = new Vector3( 5, 0.25f, 0 );
transform.position = Vector3.Slerp( transform.position, vecTarget, 0.05f );
```
