---
layout: post
title: "디지털 영상처리 - Negative"
subtitle: "ImageProcessing"
date: 2019-11-12 12:07:54
author: kwon
categories: 영상처리
---
<div style="width: 512px; height: 512px;">
    <img src="https://kyu9341.github.io/assets/ImageProcessing6.png" style="width: 512px
    ; height: 512px;">
</div>

저번 포스팅에서 위와 같이 레나 영상을 띄우는 것 까지 해보았다. 이번 포스팅부터는 여러 이미지에 대해 다양한 효과를 적용시키고 가공하는 방법에 대해 다루어볼 것이다. 우리는 그레이 스케일의 0~255 까지의 값을 가지는 이미지만 활용할 것이다.

먼저 첫번째로는 가장 기본적인 이미지 반전 효과를 주는 것에 대해 알아보겠다.

이미지를 반전시키는 방법은 간단하다. 이미지는 0~255의 값을 가지는데 각 픽셀의 가지는 값들을 모두 255에서 빼주면 된다. 다음 코드를 보자.

```c
void Negative(uchar** img, uchar** Result, int Row, int Col) // Nagative 효과 넣기 (반전)
{
	int i, j;

	for (i = 0; i < Row; i++)
		for (j = 0; j < Col; j++)
			Result[i][j] = 255 - img[i][j];

}
```
이미지를 반전시켜주는 즉, Negative효과를 주는 함수이다. 입력 이미지의 각 값들을 255에서 빼서 결과 이미지에 넣어주면 된다. 이러한 과정을 거쳐 lena이미지를 처리하면 다음과 같은 결과가 나오게 된다.

<div style="width: 512px; height: 512px;">
    <img src="https://kyu9341.github.io/assets/Negative.png" style="width: 512px
    ; height: 512px;">
</div>

위와 같이 이미지가 반전되어 출력되는 것을 확인할 수 있다.
