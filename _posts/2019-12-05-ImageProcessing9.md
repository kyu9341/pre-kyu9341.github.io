---
layout: post
title: "디지털 영상처리 - 공간 필터링"
subtitle: "ImageProcessing"
date: 2019-12-01 16:05:53
author: kwon
categories: 영상처리
---
## Spatial Filtering (공간 필터링)

영상처리에서 필터링은 영상 내에서 특별히 원하는 성분을 추출해내는 과정에 대한 용어이다. 필터링은 크게 두 가지로 나뉘는데 하나는 저주파 필터링이고 하나는 고주파 필터링이다. 필터링의 용어는 모두 주파수 영역에서 다루는 용어를 그대로 사용한다. 공간필터링이라는 용어는 영상을 입력으로 하여 출력을 영상으로 내어주는 공간상에서의 결과를 보여주는데 따른 용어이다.

### Spatial Smoothing Filtering

공간필터링의 처음은 저주파 필터링의 특징을 보여주는 Smoothing Filtering이라고도 한다. 일반적으로 평균을 취하는 형태로 그 결과를 얻을 수 있으며 주파수 상에서의 처리가 아닌 경우 지정된 필터를 사용하여 영상처리를 수행한다.



<div style="width: 800px; height: 400px;">
    <img src="https://kyu9341.github.io/assets/gaussainFiltering.png" style="width: 800px
    ; height: 400px;">
</div>

위의 영상은 가우시안 스무딩 필터를 적용하여 얻은 결과이다. 스무딩 필터의 특징은 영상 내 잡음제거가 주요한 목적이며 고주파 성분을 제거하여 화면을 부드럽게 만들어주는 것이 특징이다.

<div style="width: 400px; height: 200px;">
    <img src="https://kyu9341.github.io/assets/filter.png" style="width: 400px
    ; height: 200px;">
</div>

위의 표는 각각 가우시안 필터와 평균 필터인데 평균 필터는 모두 같은 값으로 나누는 반면, 가우시안 필터는 가운데 부분의 값을 좀 더 큰 값으로 할당하여 중심 값을 보존하면서 주변의 값에 대한 평균을 취하는 형태로, 원본 형태가 살아있는 부드러운 결과 영상을 보여준다.

<div style="width: 800px; height: 400px;">
    <img src="https://kyu9341.github.io/assets/averagelena.png" style="width: 800px
    ; height: 400px;">
</div>

위의 영상은 3x3 Average Filter를 사용한 결과 영상이며 3x3 윈도우 내의 평균값에 대한 영상의 결과의 부드러움이 아주 약하게 나타나는 것을 알 수 있다. 윈도우 크기에 따라 스무딩 효과의 차이가 크게 나타나며 이러한 결과는 어떤 필터를 사용하는가에 따라 그 결과가 확연히 달라진다.
 영상 필터링을 위한 처리에서 중요한 요소 중의 하나가 외곽 부분에 대한 처리이다. 일반적으로 영상의 외곽 부분에 대한 처리는 크게 3가지로 구분할 수 있다.
1. 주변에 0을 채워 넣어 영상 외곽 부분의 값을 0이라 가정하고 필터링을 수행
   (zero padding)
2. 대칭되는 영상이 계속적으로 존재한다는 가정 하에 영상의 외곽부분을 처리(대칭 기법)
3. 영상이 회선의 특징을 가지고 반복된다는 가정 하에 영상의 외곽부분을 처리
어떤 방법을 사용하던지 영상의 외곽 경계선 영역 몇 칸에 대한 처리이기 때문에 영상의 선명도에는 커다란 영향을 미치지 않는다. 그렇지만 최종적으로 원하는 결과를 얻기 위해서는 어떤 방법을 사용할지에 대해 고민해야 할 것이다.

다음은 각각 jet image와 livingroom image에 가우시안 필터와 평균 필터를 적용시킨 모습이다.

<div style="width: 800px; height: 400px;">
    <img src="https://kyu9341.github.io/assets/jetga.png" style="width: 800px
    ; height: 400px;">
</div>

<div style="width: 800px; height: 400px;">
    <img src="https://kyu9341.github.io/assets/livingroomGA.png" style="width: 800px
    ; height: 400px;">
</div>

가우시안 필터링과 스무딩 필터링의 차이는 필터 계수의 값에 의한 원본 영상의 영향이라고 생각하면 된다. 기본적으로 스무딩 필터는 원 영상을 뭉개는 효과가 나타나기 때문에 전체적인 모양은 흐릿한 결과를 보여주게 되는데 이러한 특성 때문에 잡음 제거를 위하여 많이 사용한다.

다음의 코드는 가우시안 필터와 평균 필터를 사용하는 코드이며 그 아래의 코드는 컨볼루션을 수행하는 코드이다. 잠깐 컨볼루션에 대해 알아보겠다.
```c
if (mod == 10) // Average nad Gaussian Filtering
	{
		printf("flag = 0 -> Gaussian Filtering\n");
		printf("flag = 1 -> Average Filtering\n");
		printf("flag : "); // 0 : Gaussian , 1 : Average
		scanf_s("%d", &flag);

		gaussMask = d_alloc(block_size, block_size);
		aveMask = d_alloc(block_size, block_size);

		//gaussian mask 설정
		gaussMask[0][0] = 1 / 16.;
		gaussMask[0][1] = 2 / 16.;
		gaussMask[0][2] = 1 / 16.;
		gaussMask[1][0] = 2 / 16.;
		gaussMask[1][1] = 4 / 16.;
		gaussMask[1][2] = 2 / 16.;
		gaussMask[2][0] = 1 / 16.;
		gaussMask[2][1] = 2 / 16.;
		gaussMask[2][2] = 1 / 16.;

		// average mask 설정
		for (i = 0; i < 3; i++)
			for (j = 0; j < 3; j++)
				aveMask[i][j] = 1 / 9.;

		printf("Start Simple Filtering \n");

		if (flag == 0)
			convolution(gaussMask, block_size, Col, Row, img, outimg);
		else if (flag == 1)
			convolution(aveMask, block_size, Col, Row, img, outimg);
		else
		{
			printf("flag must be 0 or 1 \n");
			exit(1);
		}

		printf("Finished Simple Filtering \n");


	}
```

```c

void convolution(double** h, int F_length, int size_x, int size_y, uchar** image1, uchar** image2)
{
	//  컨볼루션 계산은 마스크와 이미지 상에 대응되는 값끼리 곱하여 모두 더하여 구하며, 결과값을 결과 영상의 현재 위치에 기록
	int i, j, x, y;
	int margin, indexX, indexY;
	double sum, coeff;

	margin = (int)(F_length / 2);

	for (i = 0; i < size_y; i++)
	{
		for (j = 0; j < size_x; j++)
		{
			sum = 0;
			for (y = 0; y < F_length; y++)
			{
				indexY = i - margin + y;
				if (indexY < 0) indexY = -indexY;
				else if (indexY >= size_y) indexY = (2 * size_y - indexY - 1);

				for (x = 0; x < F_length; x++)
				{
					indexX = j - margin + x;
					if (indexX < 0) indexX = -indexX;
					else if (indexX >= size_x) indexX = (2 * size_x - indexX - 1); // 외곽 처리 (대칭 기법(시메트릭)??)

					sum += h[y][x] * (double)image1[indexY][indexX]; // 마스크의 각 값을 이미지의 픽셀 값과 곱하여 모두 더해준다.
				}
			}
//			sum += 128; // 양각 효과를 보기 위해 추가 -> 이부분만 다름

			// clipping
			if (sum < 0) sum = 0.;
			else if (sum > 255) sum = 255;
			image2[i][j] = (uchar)sum;
		}
	}
}
```
## Convolution (합성곱)

Convolution은 합성곱이라는 뜻이다. 특정한 크기의 필터를 사용하여 이미지의 각 픽셀을 지나가며 필터의 위치에 해당하는 픽셀과 모두 곱한 후 그 곱한 값을 모두 더하여 현재 중앙 픽셀 값에 넣어주는 것이다.

<div style="width: 512px; height: 256px;">
    <img src="https://kyu9341.github.io/assets/convolution1.png" style="width: 512px
    ; height: 256px;">
</div>

위의 그림처럼 이미지에서 합성곱이란, 필터를 이동시켜가며 이미지와 곱한 결과를 모두 더하는 것인데, 이러한 필터를 어떤 값을 넣어 사용하느냐에 따라 이미지의 색상, 밝기, 엣지 추출등 여러 가지 기능을 수행할 수 있다. 또한 필터는 주로 커널이라고 부르기도 한다.

<div style="width: 512px; height: 512px;">
    <img src="https://kyu9341.github.io/assets/splena.png" style="width: 512px
    ; height: 512px;">
</div>

### Median Filter (미디안 필터)

미디언 필터링은 비선형 필터로 수학적인 증명이 가능한 필터가 아니다. 수학적으로 증명이 어렵다는 이야기는 그 결과를 예측할 수 없는 이상한 방향으로 갈 수 있다는 뜻이기도 하다. 미디언 필터링이라는 용어는 영상의 중간 값을 결과로 취하기 때문에 붙여진 이름이다. 미디언 필터는 Max Filter, Min Filter와 더불어 영상 내에 존재하는 값을 이용하여 결과를 얻는 대표적인 비선형 필터이다. 영상 내에서 3x3, 혹은 5x5와 같은 윈도우 크기를 정하여 그 크기 안에 있는 영상의 순서를 가장 작은 수부터 큰 수까지 정렬을 수행하여 그 결과의 중간 값을 원하는 결과 값으로 대체하는 필터링 기법이다.


<div style="width: 800px; height: 400px;">
    <img src="https://kyu9341.github.io/assets/3355lena.png" style="width: 800px
    ; height: 400px;">
</div>



<div style="width: 800px; height: 400px;">
    <img src="https://kyu9341.github.io/assets/splenamaxmin.png" style="width: 800px
    ; height: 400px;">
</div>








<div style="width: 512px; height: 512px;">
    <img src="https://kyu9341.github.io/assets/cmancontrast.png" style="width: 512px
    ; height: 512px;">
</div>
