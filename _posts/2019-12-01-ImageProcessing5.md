---
layout: post
title: "디지털 영상처리 - Add Noise"
subtitle: "ImageProcessing"
date: 2019-12-01 16:05:53
author: kwon
categories: 영상처리
---
## Add Noise

```c

double uniform()
{
	return((double)(rand() & RAND_MAX) / RAND_MAX - 0.5);
}

double gaussian() // 가우시안 난수 생성
{
	static int ready = 0;
	static double gstore;
	double v1, v2, r, fac, gaus;
	double uniform();

	if (ready == 0)
	{
		do {
			v1 = 2. * uniform();
			v2 = 2. * uniform();
			r = v1 * v1 + v2 * v2;
		} while (r > 1.0);

		fac = sqrt(-2. * log(r) / r);
		gstore = v1 * fac;
		gaus = v2 * fac;
		ready = 1;

	}
	else
	{
		ready = 0;
		gaus = gstore;
	}

	return(gaus);
}

void AddNoise(uchar** img, uchar** outimg, int Row, int Col) // 가우시안 난수를 더해 잡음 생성
{
	int i, j;
	int temp;
	for (i = 0; i < Row; i++)
		for (j = 0; j < Col; j++) {

			outimg[i][j] = img[i][j] + gaussian() * 50;
		}

}

```

위의 코드는 가우시안 난수를 생성하는 함수와 그 함수를 이용하여 이미지에 잡음을 더해주는 함수이다. 위의 함수를 사용할 때 메인 함수에 srand(time(NULL)); 를 넣어주어야 랜덤 값이 정상적으로 발생한다. 아래는 가우시안 랜덤 잡음에 lena영상을 더하여 얻은 잡음가산 영상이다.

<div style="width: 512px; height: 512px;">
    <img src="https://kyu9341.github.io/assets/lenanoise.png" style="width: 512px
    ; height: 512px;">
</div>

영상의 원 값에 화소별로 랜덤 잡음을 더하여 그 결과 값으로 얻은 것으로 잡음이 심한 가우시안 잡음은 제거하는 것이 매우 어렵다. 영상에 더해진 잡음이 가우시안 잡음처럼 평균이 0인 값이라면 잡음 영상을 계속적으로 더해 좋은 결과 영상을 얻을 수 있다.

다음의 코드를 보자.

```c
void RmNoise(uchar** img, int** tempimg, uchar** outimg, int Row, int Col, int count) // 잡음영상을 더해 잡음 제거
{
	int i, j;
	int k;
	int temp;
	for (k = 0; k < count; k++) { // count : 잡음영상 더할 횟수
		for (i = 0; i < Row; i++)
			for (j = 0; j < Col; j++) {
				tempimg[i][j] += (img[i][j] + gaussian() * 50);
				// unsigned char 는 0~255 의 값만을 지니기 때문에
				// int형으로 선언하여 누적시킴
//				printf("%lf\n", gaussian() * 50);
			}
	}

	for (i = 0; i < Row; i++)
		for (j = 0; j < Col; j++) {
			temp = tempimg[i][j] / count; // 누적 값을 평균내어 temp에 저장
//			printf("temp : %d\n", temp);

			if (temp < 0) // clipping
			{
				temp = 0;
				outimg[i][j] = temp;
			}
			else if (temp > 255)
			{
				temp = 255;
				outimg[i][j] = temp;
			}
			else
			{
				outimg[i][j] = temp;
			}
//			printf("outimg[i][j] : %d\n", outimg[i][j]);
		}

}

```

위의 코드는 잡음 영상을 입력받은 count값만큼 더하여 평균을 낸 영상을 구하는 함수이다. 다음은 위의 함수를 사용하여 각각 잡음 5개, 20개, 50개를 더한 영상이다.

<div style="width: 512px; height: 512px;">
    <img src="https://kyu9341.github.io/assets/noise5.png" style="width: 512px
    ; height: 512px;">
</div>

<div style="width: 512px; height: 512px;">
    <img src="https://kyu9341.github.io/assets/noise20.png" style="width: 512px
    ; height: 512px;">
</div>

<div style="width: 512px; height: 512px;">
    <img src="https://kyu9341.github.io/assets/noise50.png" style="width: 512px
    ; height: 512px;">
</div>
