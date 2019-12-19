---
layout: post
title: "디지털 영상처리 - 콘트라스트 스트레칭(Contrast Streching)"
subtitle: "ImageProcessing"
date: 2019-12-01 16:05:53
author: kwon
categories: 영상처리
---
## 콘트라스트 스트레칭
콘트라스트 스트레칭은 히스토그램의 분포가 얼마나 넓게 퍼져 있는가에 따라 인간의 시각 체계가 선명하게 영상을 인식하는 과정에 대한 작업이다. 밝은 화소와 어두운 화소들의 분포가 고르게 퍼져 있어야 인간은 영상을 선명하게 인식하므로 영상의 화소 분포가 좀 더 넓은 영역에 걸쳐서 퍼지도록 스트레칭 시키는 작업이다. 히스토그램 평활화는 히스토그램의 전체적인 분포를 중심으로 동작하나 콘트라스트 스트레칭은 콘트라스트가 너무 높거나 낮은 영상에 대하여 콘트라스트를 넓게 펴주는 작업을 수행하는 히스토그램 평활화의 일종이라고 보면 된다.
콘트라스트 스트레칭을 수행하는 수식은 아래와 같이 정의한다.

<div style="width: 512px; height: 80px;">
    <img src="https://kyu9341.github.io/assets/contrast.png" style="width: 512px
    ; height: 80px;">
</div>

새로운 화소 값은 영상내의 히스토그램을 구한 결과 값에서 가장 큰 값과 가장 작은 값을 구한 후, 기존의 화소 값에서 가장 작은 값을 뺀 결과에 최댓값을 곱해서 구한다. 이 때 최댓값과 최솟값의 범위가 좁을수록 콘트라스트 스트레칭이 잘 일어난다. 콘트라스트 스트레칭은 일반 영상에 적용하는 경우, 즉 최솟값이 0이고 최댓값이 255인 경우 효과를 보기 어려운 기법이다.


<div style="width: 512px; height: 512px;">
    <img src="https://kyu9341.github.io/assets/light.png" style="width: 512px
    ; height: 512px;">
</div>

<div style="width: 512px; height: 512px;">
    <img src="https://kyu9341.github.io/assets/lightcontrast.png" style="width: 512px
    ; height: 512px;">
</div>

<div style="width: 512px; height: 512px;">
    <img src="https://kyu9341.github.io/assets/lightcontrasthisto.png" style="width: 512px
    ; height: 512px;">
</div>

<div style="width: 512px; height: 512px;">
    <img src="https://kyu9341.github.io/assets/cmancontrast.png" style="width: 512px
    ; height: 512px;">
</div>



<div style="width: 512px; height: 512px;">
    <img src="https://kyu9341.github.io/assets/jetcontrast.png" style="width: 512px
    ; height: 512px;">
</div>





<div style="width: 512px; height: 512px;">
    <img src="https://kyu9341.github.io/assets/cmancontrast.png" style="width: 512px
    ; height: 512px;">
</div>


```c

void contrastStreching(uchar** img, uchar** outimg, int X_Size, int Y_Size)
{
	int i, j, histogram[256];
	int min = 255, max = 0;
	uchar LUT[256];
	double scaleFactor, tmp;
	for (i = 0; i < 256; i++)
		histogram[i] = 0;

	for(i = 0; i < Y_Size; i++)
		for (j = 0; j < X_Size; j++)
		{
			histogram[img[i][j]]++; // 영상의 히스토그램을 구함
		}
	for(i = 0; i < 256; i++) // 히스토그램의 최솟값을 구함
		if (histogram[i])
		{
			min = i;
			break;
		}
	for(i = 255; i >= 0; i--) // 히스토그램의 최댓값을 구함
		if (histogram[i])
		{
			max = i;
			break;
		}

	printf(" Low Threshold is %d High Threshold is %d\n", min, max);
	for (i = 0; i < min; i++)
		LUT[i] = 0;
	for (i = 255; i >= 0; i--)
		LUT[i] = 255;

	scaleFactor = 255.0 / (double)(max - min);

	for (i = min; i < max; i++)
	{
		tmp = (i - min) * scaleFactor;

		if (tmp < 0) tmp = 0;
		if (tmp > 255) tmp = 255;
		LUT[i] = (uchar)tmp;
	}

	for (i = 0; i < Y_Size; i++)
		for (j = 0; j < X_Size; j++)
			outimg[i][j] = LUT[img[i][j]];

}
```

















<div style="width: 512px; height: 512px;">
    <img src="https://kyu9341.github.io/assets/cmancontrast.png" style="width: 512px
    ; height: 512px;">
</div>
