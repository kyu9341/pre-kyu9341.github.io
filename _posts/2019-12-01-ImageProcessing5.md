---
layout: post
title: "디지털 영상처리 - Bit Plane"
subtitle: "ImageProcessing"
date: 2019-11-29 11:01:22
author: kwon
categories: 영상처리
---
## Bit Plane

표준 디지털 영상은 8bits로 구성되어 0부터 255까지의 값을 가지고 있는 구조이다. 256레벨의 그레이 스케일 값이라고 표현하기도 하는데 이진영상을 위에서 구성한 것과 같이 각 bit위치에서의 영상을 독립된 영상으로 표현하는 방법을 알아본다. 영상은 8개의 bit plane으로 구성되어 있으며 최상위 비트가 1인 경우 128보다 큰 값을 가지게 되고 바로 하위 비트가 1인 경우는 64보다 큰 값을 갖는 구조를 가지고 있다. 각 위치에서의 비트가 1인 경우 영상 값이 존재하는 표시인 255로 표시하여 화면에 보여주고 0인 경우 0으로 표현하여 이진 영상으로 각 bit plane을 표현한다.

```c

void BitSlicing(uchar** img, uchar** Result, int Row, int Col, int position)
{
  // 해당 position 값에 맞는 mask값을 통해 이미지를 구함

	int i, j;
	uchar mask = 0x01;
	mask <<= position;

	for(i=0;i<Row;i++)
		for (j = 0; j < Col; j++) {
			if ((mask & img[i][j]) == pow(2, position)) //pow(2, position) = mask
			{
				Result[i][j] = 255;
			}
			else
			{
				Result[i][j] = 0;
			}
		}
}

```

위의 코드는 position값에 따라 해당하는 bit palne의 이미지를 생성하는 코드이다.

<div style="width: 700px; height: 800px;">
    <img src="https://kyu9341.github.io/assets/lenabitplane.png" style="width: 700px
    ; height: 800px;">
</div>

위의 이미지는 Bit Plane 8장에 대한 영상결과를 보여준다. 각각의 비트가 1인 경우 255로 표현하여 이진화된 영상을 보여준다.

##ImageCombine

이진화된 비트 영상은 비트의 정보만으로 영상이 얼마나 많은 정보를 가지고 있는지를 알 수 있다. 이러한 비트 정보를 몇 개를 모아야 사람이 보기에 지장이 없는지 확인해보자.

```c
void ImageCombine(uchar** img, uchar** tmpimg, uchar** outimg, int Row, int Col, int position, int direction) // 7부터 position까지의 비트이미지를 합성
{
	int i, j, k;
	uchar mask = 0x01;

	if (direction == 1) // 7부터 position까지 합성
	{

		for (k = 7; k > 7 - position; k--) // 7부터 입력받은 장수까지 반복
		{
			mask <<= k;
			for (i = 0; i < Row; i++) // BitSlicing
				for (j = 0; j < Col; j++) {
					if ((mask & img[i][j]) == mask) //pow(2, position) = mask
					{
						tmpimg[i][j] = 255;
					}
					else
					{
						tmpimg[i][j] = 0;
					}

				}
			for (i = 0; i < Row; i++)
				for (j = 0; j < Col; j++) {
					outimg[i][j] += tmpimg[i][j] & mask;
				}
			mask = 0x01;
		}
		average(outimg, Row, Col);

	}
/*	 비트슬라이싱된 이미지에 해당 mask를 and연산 하는 이유
	 해당 비트값만 255로 변환하고 나머지는 0으로 변환했기 때문에
	 즉 255 : 0b1111111 & 0b1000000 => 0b1000000
			    & 0b0100000 => 0b0100000
						와 같이 변환 후 모두 더해줘야 정상적인 값이 나옴.
\*/
	if (direction == 2) // 1부터 position까지 합성
	{

		for (k = 1; k <= position; k++) // 1부터 입력받은 장수까지 반복
		{
			mask <<= k;
			for (i = 0; i < Row; i++) // BitSlicing
				for (j = 0; j < Col; j++) {
					if ((mask & img[i][j]) == mask) //pow(2, position) = mask
					{
						tmpimg[i][j] = 255;
					}
					else
					{
						tmpimg[i][j] = 0;
					}

				}
			for (i = 0; i < Row; i++) // BitMasking, MaskAdd
				for (j = 0; j < Col; j++) {
					outimg[i][j] += tmpimg[i][j] & mask; // 비트슬라이싱된 tmpimg를 해당 비트의 마스크 값으로 마스킹 후 누적
				}
			mask = 0x01; // 초기화
		}
		average(outimg, Row, Col);

	}

}
```
위의 코드는 입력 받은 direction과 position값에 따라 상위 비트 혹은 하위 비트부터 원하는 장수를 더한 이미지를 생성해주는 함수이다. 일일이 각 비트의 이미지를 생성해 더하지 않도록 하기 위해 위와 같이 mask를 쉬프트 연산을 통해 변경하며 각각의 bit에 해당하는 이미지를 누적시켰다. 아래는 위의 함수를 통해 생성한 이미지를 출력한 결과이다.

<div style="width: 512px; height: 512px;">
    <img src="https://kyu9341.github.io/assets/lena876.png" style="width: 512px
    ; height: 512px;">
</div>
8, 7, 6bit 합성 영상

<div style="width: 512px; height: 512px;">
    <img src="https://kyu9341.github.io/assets/lena8765.png" style="width: 512px
    ; height: 512px;">
</div>
8, 7, 6, 5bit 합성 영상

<div style="width: 512px; height: 512px;">
    <img src="https://kyu9341.github.io/assets/lena12345.png" style="width: 512px
    ; height: 512px;">
</div>
1, 2, 3, 4, 5bit 합성 영상

상위 비트부터 많은 비트를 합성할수록 원 영상과 비슷한 결과를 보여주는 것을 알 수 있다. 또한 하위 비트부터 더하면 높은 값을 가지는 bit가 빠져있으므로 전체적으로 어두운 영상을 보여주게 된다.
