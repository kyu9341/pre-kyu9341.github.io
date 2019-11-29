---
layout: post
title: "디지털 영상처리 - Bit Plane"
subtitle: "ImageProcessing"
date: 2019-11-29 11:01:22
author: kwon
categories: 영상처리
---
## Bit Plane

표준 디지털 영상은 8bits로 구성되어 0부터 255까지의 값을 가지고 있는 구조이다. 256레벨의 그레이 스케일 값이라고 표현하기도 하는데 이진영상을 위에서 구성한 것과 같이 각 bit위치에서의 영상을 독립된 영상으로 표현하는 방법을 알아본다. 영상은 8개의 bit plane으로 구성되어 있으며 최상위 비트가 1인 경우 128보다 큰 값을 가지게 되고 바로 하위 비트가 1인 경우는 64보다 큰 값을 갖는 구조를 가지고 있다. 각 위치에서의 비트가 1인 경우 영상 값이 존재하는 표시인 255로 표시하여 화면에 보여주고 0인 경우 0으로 표현하여 이진 영상으로 각 bit plane을 표현한다.

<div style="width: 512px; height: 512px;">
    <img src="https://kyu9341.github.io/assets/lenabitplane.png" style="width: 512px
    ; height: 512px;">
</div>




















참조 : <>
