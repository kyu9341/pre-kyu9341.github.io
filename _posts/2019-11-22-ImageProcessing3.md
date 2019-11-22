---
layout: post
title: "디지털 영상처리 - Binary Image"
subtitle: "ImageProcessing"
date: 2019-11-22 21:02:31
author: kwon
categories: 영상처리
---
## Binary Image

영상정보는 0에서 255까지의 값을 가지고 있어서 필요에 따라 영상의 값들을 조작하거나 변경하는 영상처리를 수행한다. 그러나 컴퓨터 시스템의 특성과 영상처리의 단순화를 위하여 이진화 하는 작업이 필요한 경우가 많이 존재한다. 영상 이진화는 0에서 255까지의 값을 0과 1의 값으로 변환하는 작업이다. 실제 영상에서는 0과 1 모두 검은 값으로 표시되기 때문에 1은 255로 변환하여 표시하여 준다. 다음 이미지는 영상의 평균 값을 구하여 평균보다 작은 값은 0으로, 평균 이상의 값들은 모두 255로 변환하여 출력한 결과이다.

<div style="width: 512px; height: 512px;">
    <img src="https://kyu9341.github.io/assets/binary.png" style="width: 512px
    ; height: 512px;">
</div>

위의 이미지는 lena 영상을 평균 값을 기준으로 이진화를 시킨 모습이다. 이미지의 평균값을 구하는 함수와 평균값을 기준으로 영상을 이진화하는 함수는 다음 코드와 같다.

```c
double average(uchar** img, int Row, int Col) // 이미지의 전체 평균값을 구하는 함수
{
	double sum = 0, avg;
	int i, j;

	for (i = 0; i < Row; i++) {
		for (j = 0; j < Col; j++) {
			sum += img[i][j];
		}
	}
	avg = sum / ((double)Row * Col);
	printf("Average of Image %1f \n", avg);
	return avg;

}

void makeBinary(uchar** img, uchar** out, int Row, int Col, double avg) // 이미지 2진화
{
	int i, j;
	for (i = 0; i < Row; i++) {
		for (j = 0; j < Col; j++) {
			if (img[i][j] > avg) out[i][j] = 255;
			else out[i][j] = 0; // 평균보다 큰 값은 255, 작은 값은 0 대입

		}
	}
}
```
이진화 영상은 평균값 이외에도 특정한 threshold 값을 지정함으로써 원하는 형태의 이진화 영상을 구할 수 있다. 각각의 영상처리 역할과 분야에 따라 이진화 영상은 다양한 방법으로 사용될 수 있다.


```c
void AdaptiveBinary2(uchar** img, uchar** out, int Row, int Col)
{
	int i, j;

	for (i = 0; i < Row; i++) {
		for (j = 0; j < Col; j++) {
			if (img[i][j] > 50 && img[i][j] < 120) out[i][j] = img[i][j];
			else out[i][j] = 0;
		}
	}
}

void AdaptiveBinary1(uchar** img, uchar** out, int Row, int Col)
{
	int i, j;

	for (i = 0; i < Row; i++) {
		for (j = 0; j < Col; j++) {
			if (img[i][j] > 50 && img[i][j] < 120) out[i][j] = 200;
			else out[i][j] = img[i][j];
		}
	}
}


void AdaptiveBinary0(uchar** img, uchar** out, int Row, int Col)
{
	int i, j;

	for (i = 0; i < Row; i++) {
		for (j = 0; j < Col; j++) {
			if (img[i][j] > 50 && img[i][j] < 120) out[i][j] = 200;
			else out[i][j] = 0;
		}
	}
}
```
위의 코드는 다양한 방식의 이진화 영상을 구하는 방법이다. 기준치를 다양하게 주어 각 기준치마다 영상의 값을 지정해줄 수도 있다.

다음은 위의 jet 영상을 각각 AdaptiveBinary0, 1, 2에 적용시킨 모습이다.

<div style="width: 512px; height: 512px;">
    <img src="https://kyu9341.github.io/assets/jet.png" style="width: 512px
    ; height: 512px;">
</div>
**jet512.dat 원영상**

<div style="width: 512px; height: 512px;">
    <img src="https://kyu9341.github.io/assets/jet0.png" style="width: 512px
    ; height: 512px;">
</div>
**AdaptiveBinary0**

<div style="width: 512px; height: 512px;">
    <img src="https://kyu9341.github.io/assets/jet1.png" style="width: 512px
    ; height: 512px;">
</div>
**AdaptiveBinary1**

<div style="width: 512px; height: 512px;">
    <img src="https://kyu9341.github.io/assets/jet2.png" style="width: 512px
    ; height: 512px;">
</div>
**AdaptiveBinary2**
