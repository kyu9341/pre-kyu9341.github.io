---
layout: post
title: "디지털 영상처리 - 콘트라스트 스트레칭(Contrast Streching)"
subtitle: "ImageProcessing"
date: 2019-12-01 16:05:53
author: kwon
categories: 영상처리
---

<div style="width: 512px; height: 80px;">
    <img src="https://kyu9341.github.io/assets/contrast.png" style="width: 512px
    ; height: 80px;">
</div>



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
