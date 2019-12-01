---
layout: post
title: "디지털 영상처리 - 부분 Mosaic"
subtitle: "ImageProcessing"
date: 2019-12-01 16:05:53
author: kwon
categories: 영상처리
---
## 논리 연산
영상을 처리하기 위하여 단순하게 수를 더하거나 빼는 연산만을 수행하는 것이 아니라 영상에 대하여 논리적인 연산을 수행하여 원하는 결과를 얻을 수 있다.

```c
void Circle(uchar** Result, int Row, int Col, double diameter) // 원하는 반지름의 크기로 원 생성
{
	int i, j;
	double tmp, xSquare, ySquare;

	for(i=0;i<Row;i++)
		for (j = 0; j < Col; j++)
		{
			ySquare = (abs(Row / 2 - i)) * (abs(Row / 2 - i)); // (Row/2, Col/2) 는 중심의 좌표
			xSquare = (abs(Col / 2 - j)) * (abs(Col / 2 - j));

			tmp = sqrt(ySquare + xSquare); // tmp는 현재 위치의 중심과의 거리 - 피타고라스 정리 x^2 + y^2 = z^2
			//sqrt() : 제곱근을 구하는 함수
			if (tmp < diameter) Result[i][j] = 255; //
			else Result[i][j] = 0;
		}
}

```
위의 코드는 반지름의 길이를 매개변수로 받아 원하는 반지름을 가지는 원을 생성해주는 함수이다. 중심과의 거리를 이용하여 반지름보다 큰 범위의 값은 모두 0으로 작은 값은 모두 255로 변환하게 된다.

<div style="width: 512px; height: 512px;">
    <img src="https://kyu9341.github.io/assets/circle100.png" style="width: 512px
    ; height: 512px;">
</div>

위의 이미지는 반지름이 100인 circle image이다. 다음은 두 이미지를 인자로 받아 and 연산과 or 연산을 수행해주는 함수이다.

```c

void MaskOr(uchar** in1Img, uchar** in2Img, uchar** outImg, int Row, int Col) // or 연산
{
	int i, j;

	for (i = 0; i < Row; i++)
		for (j = 0; j < Col; j++) {
			outImg[i][j] = in1Img[i][j] | in2Img[i][j];
		}
}
void MaskAnd(uchar** in1Img, uchar** in2Img, uchar** outImg, int Row, int Col) // and 연산
{
	int i, j;

	for (i = 0; i < Row; i++)
		for (j = 0; j < Col; j++) {
			outImg[i][j] = in1Img[i][j] & in2Img[i][j];
		}
}
```
위의 함수들을 이용하여 lena영상의 얼굴 부분만을 구할 수 있다.

<div style="width: 512px; height: 512px;">
    <img src="https://kyu9341.github.io/assets/lenaface.png" style="width: 512px
    ; height: 512px;">
</div>
위의 이미지는 lena와 circle을 AND연산을 수행하여 얻은 결과이다.(diameter=150)
<div style="width: 512px; height: 512px;">
    <img src="https://kyu9341.github.io/assets/lenaor.png" style="width: 512px
    ; height: 512px;">
</div>
위의 이미지는 lena와 circle을 OR연산을 수행하여 얻은 결과이다.

이를 응용하면 원하는 부분만 모자이크 처리를 수행하는 것이 가능하다. 먼저 lena와 circle이미지를 OR연산을 수행한 뒤 앞서 해보았던 mosaic 함수를 이용하여 모자이크된 lena영상과 AND연산을 수행하면 된다. 하지만 위와 같은 방법을 사용하면 메모리를 여러 개 할당해야하는 불편함과 위치를 원하는 곳으로 지정할 수 없는 불편함 있기 때문에 다음과 같은 함수를 작성하였다.

```c
void CircleMosaic(uchar** img, uchar** outimg, int Row, int Col, int x, int y, double diameter, int block)
{
	int i, j, a, b;
	double tmp, xSquare, ySquare, avg = 0;
	int mtmp, count;

	mosaic(img, outimg, Row, Col, block); // outimg에 모자이크된 이미지 저장

	for(i = 0; i < Row; i++)
		for (j = 0; j < Col; j++)
		{
			ySquare = (y - j) * (y - j);
			xSquare = (x - i) * (x - i);

			tmp = sqrt(ySquare + xSquare); // 피타고라스 정리 -> 중심과의 거리 찾기

			if (tmp < diameter) outimg[i][j]; // 현재 픽셀의 위치가 입력받은 반지름보다 작으면 모자이크된 이미지를 유지
			else outimg[i][j] = img[i][j]; // 그렇지 않으면 원본이미지를 대입
		}

}
```
위의 함수는 원하는 좌표와 반지름을 매개변수로 받아 해당하는 위치에 모자이크 처리를 수행하는 함수이다. mosaic함수는[이전 포스팅](https://kyu9341.github.io/%EC%98%81%EC%83%81%EC%B2%98%EB%A6%AC/2019/11/12/ImageProcessing2.html)에서 사용했던 함수이고 circle이미지를 제작할 때 사용했던 방식으로 작성하였다. 위의 함수를 이용하면 다음

<div style="width: 512px; height: 512px;">
    <img src="https://kyu9341.github.io/assets/partMosaic.png" style="width: 512px
    ; height: 512px;">
</div>
