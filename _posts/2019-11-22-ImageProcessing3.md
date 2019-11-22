---
layout: post
title: "디지털 영상처리 - Binary Image, Gamma Correction"
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
위의 코드는 다양한 방식의 이진화 영상을 구하는 방법이다. 기준치를 다양하게 주어 각 기준치마다 영상의 값을 지정해줄 수도 있다. 이러한 기법은 특정한 목적을 가지고 영상을 변환하고자 할 때 적용할 수 있는 방법이다.

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

## Gamma Correction

Gamma Correction은 음극선관을 사용하는 CRT(carhode ray tube) 모니터나 텔레비전에서 영상을 제대로 보여줄 때 하드웨어적인 문제점을 보완하기 위해 도입된 기법으로 영상의 전체적인 밝기를 조절하는 방법이다.

<div style="width: 30%; height: 100px;">
    <img src="https://kyu9341.github.io/assets/gammamath.png" style="width:100%
    ; height: 100px;">
</div>
위의 수식에서 r값이 1을 기준으로 작을수록 영상이 어두워지고, 클수록 영상이 밝아지게 된다.

<div style="width: 100%; height: 200px;">
    <img src="https://kyu9341.github.io/assets/gamma.png" style="width:100%
    ; height: 200px;">
</div>

위의 히스토그램과 같이 gamma(r)값이 1보다 작다면 영상이 가지는 어두운 값이 많고 1보다 크다면 영상이 가지는 밝은 값이 많아져 위의 히스토그램과 같은 분포를 가지게 된다. gamma값으로 영상의 밝기를 변환해주는 함수는 다음과 같다.

```c
void PowImg(uchar** img, uchar** Result, int Row, int Col, double gamma) // gamma correction image
{
	// 영상의 전체적인 밝기를 조절 (gamma 값에 따라 - gamma가 클수록 밝아짐)
	int i, j;
	double tmp;

	for(i=0; i< Row; i++)
		for (j = 0; j < Col; j++) {
			tmp = pow(img[i][j] / 255., 1 / gamma); // pow(a, b) -> a^b 반환
			// 지수법칙 변환 범위 : 0 ~ 1 -> 스케일링 필요 : s = [(r/255)^(1/r)] * 255

			if (tmp * 255 > 255) tmp = 1; //
			else if (tmp * 255 < 0) tmp = 0;

			Result[i][j] = tmp * 255;
		}
}
```

다음은 각각 r값을 0.5, 3을 주어 밝기를 변환한 영상이다. lena원영상의 픽셀값의 평균은 123.607670이다. 변환된 이미지의 평균값과 비교해보자.

<div style="width: 512px; height: 512px;">
    <img src="https://kyu9341.github.io/assets/gamma0.5.png" style="width: 512px
    ; height: 512px;">
</div>
gamma = 0.5, 평균 = 68.473244

<div style="width: 512px; height: 512px;">
    <img src="https://kyu9341.github.io/assets/gamma3.png" style="width: 512px
    ; height: 512px;">
</div>
gamma = 3, 평균 = 195.887569

gamma값에 따라 확연히 밝기가 달라지고 평균값 또한 밝기에 따라 함께 변화하는 것을 확인할 수 있다. 이러한 기법은 영상을 밝게 혹은 어둡게 해야 더 많은 정보를 획득할 수 있는 경우 중요한 기법으로 사용할 수 있다.

추가적으로 원하는 평균값을 가지는 영상을 구하는 방법을 알아보자. 예를 들어, lena영상으로 원하는 평균값을 가지는 영상으로 변환하고 싶다면 어떻게 해야할까? 다음의 코드롤 보자.


```c
int hopeAvg;
printf("원하는 평균값 입력 : ");
scanf_s("%d", &hopeAvg);
// 원하는 평균값 구하기
if (msum < hopeAvg) { // 원본 이미지의 평균값이 원하는 평균값 보다 작은 경우
  for (gamma = 1; gamma < 4; gamma += 0.005) { // 연산량을 줄이기 위해 gamma = 1(원본 이미지) 부터 시작
    PowImg(img, outimg, Row, Col, gamma);
    msum = average(outimg, Row, Col);
    printf("gamma = %f\n", gamma);

    if (msum >= hopeAvg)
      break;
  }
}
else // 원본 이미지의 평균값이 원하는 평균값 이상인 경우
{
  for (gamma = 1; gamma > 0; gamma -= 0.005) { // 연산량을 줄이기 위해 gamma = 1(원본 이미지) 부터 시작
    PowImg(img, outimg, Row, Col, gamma);
    msum = average(outimg, Row, Col);
    printf("gamma = %f\n", gamma);

    if (msum < hopeAvg)
      break;

  }
}
```
원하는 평균값을 입력받아 gamma값을 키우거나, 줄이며 원하는 평균값이 나오면 break로 반복문을 빠져나오면 된다. 이러한 방식으로 부르트 포스 알고리즘을 적용하여 해결할 수 있다. 부르트 포스 알고리즘이란, 모든 경우의 수를 다 해보며 원하는 값을 찾는 것인데 이러한 방식은 시간이 오래걸리므로 조금이라도 연산 시간을 줄이기 위해 gamma값을 1부터 시작하도록 하였다. 원영상의 gamma값은 1을 가지며 원하는 평균이 1보다 작다면 1부터 줄여가고, 1보다 크다면 1부터 키워가는 방식이다. 원하는 값에 도달하면 반복을 멈추고 이미지를 생성한다.

이러한 방식으로 평균값 151을 가지는 레나 영상을 구해보았다.

<div style="width: 100%; height: 400px;">
    <img src="https://kyu9341.github.io/assets/gamma151.png" style="width: 70%
    ; height: 400px;">
</div>
위와 같이 지정한 평균값에 도달하면 반복을 멈추고 이미지를 생성한다.

<div style="width: 512px; height: 512px;">
    <img src="https://kyu9341.github.io/assets/gamma151img.png" style="width: 512px
    ; height: 512px;">
</div>

평균값 151을 가지는 lena영상이다.




참조 : <http://blog.daum.net/_blog/BlogTypeView.do?blogid=050RH&articleno=12109204&categoryId=44&regdt=20130604182910>
