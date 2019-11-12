---
layout: post
title: "디지털 영상처리 - Negative, Mosaic"
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

## Negative(반전 효과)

먼저 첫번째로는 가장 기본적인 이미지 반전 효과를 주는 것에 대해 알아보겠다.

이미지를 반전시키는 방법은 간단하다. 이미지는 0~255의 값을 가지는데 각 픽셀의 가지는 값들을 모두 255에서 빼주면 된다. 다음 코드를 보자.

```c
typedef unsigned char uchar;


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

## Mosaic(모자이크)

다음은 이미지에 모자이크 효과를 주는 방법을 알아보겠다.

모자이크는 먼저 모자이크 블록의 크기를 입력받아 그 크기만큼 증가하며 이미지의 한 블록 내의 값들의 평균을 내어 그 블록의 값을 모두 평균값으로 바꿔주면 된다. 아래의 코드를 보자.

```c

void mosaic(uchar** img, uchar** out, int Row, int Col, int Block) // 이미지 모자이크 처리
{
	int i, j, x, y, tmp, count;
	for (i = 0; i < Row; i += Block) // 입력 받은 블록 크기만큼 증가하며 반복
	{
		for (j = 0; j < Col; j += Block)
		{
			tmp = 0;
			count = 0;
			for (y = 0; y < Block; y++) // 블록 크기만큼 반복
			{
				for (x = 0; x < Block; x++) {
					tmp += img[i + y][j + x]; // uchar형에서는 0~255 값밖에 처리 못하므로 정수형 변수에 각 픽셀값을 누적
					count++; // 몇번 누적했는지 측정
				}
			}
			tmp /= count; // 블록 내의 평균을 구함
			for (y = 0; y < Block; y++)
			{
				for (x = 0; x < Block; x++) {
					out[i + y][j + x] = tmp; // 해당 블록의 각 위치에 모두 평균값을 대입
				}

			}
		}
	}
}

```
위와 같이 구현할 수 있으며 다음 이미지는 lena를 각각 블록 크기를 8, 16으로 하여 모자이크 처리를 한 이미지이다.

<div style="width: 512px; height: 512px;">
    <img src="https://kyu9341.github.io/assets/mosaic8.png" style="width: 512px
    ; height: 512px;">
</div>


<div style="width: 512px; height: 512px;">
    <img src="https://kyu9341.github.io/assets/mosaic16.png" style="width: 512px
    ; height: 512px;">
</div>

위와 같이 지정한 블록 크기에 따라 모자이크의 정도가 달라지는 것을 볼 수 있다.
