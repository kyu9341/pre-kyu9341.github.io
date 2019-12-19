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
위의 영상은 밝은 레나 영상에 콘트라스트 스트레칭을 적용한 결과이다. 밝은 레나 영상의 최솟값은 115, 최댓값은 248이다. 최솟값이 큰 값을 가지고 있어 히스토그램이 밝은 영역으로 치우친 영상의 모양으로 콘트라스트 스트레칭 결과 영상이 보기에도 선명하고 좋은 결과를 보여주는 것을 알 수 있다.

<div style="width: 512px; height: 512px;">
    <img src="https://kyu9341.github.io/assets/lightcontrasthisto.png" style="width: 512px
    ; height: 512px;">
</div>

위의 히스토그램을 보면 알 수 있듯이 최솟값과 최댓값이 스트레칭되어 각각 0과 255를 가지도록 변화한 것을 볼 수 있다.

<div style="width: 512px; height: 256px;">
    <img src="https://kyu9341.github.io/assets/cmancontrast.png" style="width: 512px
    ; height: 256px;">
</div>



<div style="width: 512px; height: 256px;">
    <img src="https://kyu9341.github.io/assets/jetcontrast.png" style="width: 512px
    ; height: 256px;">
</div>
위의 영상들은 각각 카메라맨과 제트기 영상에 대하여 콘트라스트 스트레칭을 수행한 결과이다. 카메라맨 영상은 0부터 255까지의 값을 모두 가지고 있어 그 결과가 거의 변한 것이 없으나 제트기 영상은 15부터 231의 값을 가지므로 원 영상보다 선명한 결과를 보여준다. 콘트라스트 스트레칭은 영상이 가지는 그레이 레벨의 값이 0부터 255로 변화되며 영상이 가지는 그레이 레벨 값을 모두 펼쳐주는 역할을 한다.


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

위의 코드는 콘트라스트 스트레칭을 수행하는 함수이며 영상내의 히스토그램을 구해 최솟값과 최댓값을 각각 구해 위의 수식을 적용시키는 구조로 작성되었다.
