---
layout: post
title: "디지털 영상처리"
subtitle: "ImageProcessing"
date: 2019-11-11 23:09:13
author: kwon
categories: 영상처리
---
이번 학기에 배우고 있는 과목인 디지털 영상처리를 리뷰해보겠다. 원래는 영상처리를 블로그에 리뷰할지 말지 고민을 했었는데 점점 재미있고 더 깊이 알아보고 싶어서 앞으로 지금까지 해왔던 과정이나 앞으로 진행하는 과정을 하나씩 리뷰해볼 생각이다.

우선 교수님이 이 수업의 목적은 C 프로그래밍 능력 향상이라 하셨다. 그 목적에 맞게 영상을 보기 위한 프로그램은 OpenCV를 사용하며 그 외의 소스는 순수하게 C언어 코드로만 영상처리를 수행한다.

이번 포스팅에서는 간단히 작업 환경 설정과 영상처리의 대표 이미지인 레나이미지를 띄워보는 것 까지 진행해보겠다.

작업 환경은 윈도우의 비쥬얼 스튜디오에서 진행하며 OpenCV 3.4.1 버전을 사용한다.

다운로드 주소 : <https://sourceforge.net/projects/opencvlibrary/files/opencv-win/3.4.1/opencv-3.4.1-vc14_vc15.exe/download>

원하는 경로에 다운로드를 받고 환경변수를 설정해주도록 한다. 경로는 opencv\build\x64\vc15\bin와 같고 앞에 자신이 저장한 폴더의 경로를 추가하면 된다.

다음으로는 프로젝트를 생성하고 프로젝트 이름을 opencv라고 하겠다. 이후 프로젝트를 우클릭하여 속성으로 이동해 다음과 같이 설정을 변경한다.

<div style="width: 100%; height: 500px;">
    <img src="https://kyu9341.github.io/assets/ImageProcessing1.png" style="width: 100%
    ; height: 500px;">
</div>

위에 표시된 부분을 확인하고 구성의 Debug와 Release를 각각 선택하여 플랫폼을 x64로 설정한다. 모든 설정은 Debug와 Release모두 설정해주도록 한다.

다음은 링커의 일반에서 다음과 같이 설정한다.

<div style="width: 100%; height: 400px;">
    <img src="https://kyu9341.github.io/assets/ImageProcessing2.png" style="width: 100%
    ; height: 500px;">
</div>

이 후 링커의 입력 부분으로 가서 추가 종속성에 오른쪽 끝의 화살표를 누르면 편집(Edit)이 뜨고 편집화면에 Debug인 경우 opencv_world341d.lib를 입력하고 적용을 누르며 Release인 경우 opencv_world341.lib를 입력하고 적용을 해준다.
00%; height: 400px;">
    <img src="https://kyu9341.github.io/assets/ImageProcessing3.png" style="width: 100%
    ; height: 500px;">
</div>

위의 사진이 340인 이유는 내가 OpenCV 3.4.0버전을 받았기 때문이다. 별 차이는 없으니 이어서 진행하자.

마지막으로 VC++디렉터리 오른쪽의 포함 디렉토리에 아래와 같이 include디렉토리와 라이브러리 디렉토리를 설정해준다.

<div style="width: 100%; height: 500px;">
    <img src="https://kyu9341.github.io/assets/ImageProcessing4.png" style="width: 100%
    ; height: 500px;">
</div>

아래 프로그램은 C언어로 Raw Image를 화면에 띄우는 프로그램이다. OpenCV를 이용하였고 앞으로 이미지를 띄울 때 이 프로그램을 계속해서 사용할 것이다.

```C
#include <opencv\cv.h>
#include <opencv\highgui.h>
#include <opencv\cxcore.h>

#include <stdio.h>
typedef unsigned char uchar;

//#define unsigned char uchar

unsigned char** uc_alloc(int size_x, int size_y) {
	unsigned char** m;
	int i;

	if ((m = (unsigned char**)calloc(size_y, sizeof(unsigned char*))) == NULL) {

		printf("uc_alloc error 1\7\n");
		exit(0);
	}

	for (i = 0; i < size_y; i++)
		if ((m[i] = (unsigned char*)calloc(size_x, sizeof(unsigned char))) == NULL) {
			printf("uc_alloc error 2\7\n");
			exit(0);

		}

	return m;

}

void read_ucmartrix(int size_x, int size_y, unsigned char** ucmatrix, char* filename) {
	int i;
	FILE* f;

	if ((fopen_s(&f, filename, "rb")) != NULL) {
		printf("%s File open Error!\n", filename);
		exit(0);

	}

	for (i = 0; i < size_y; i++)
		if (fread(ucmatrix[i], sizeof(unsigned char), size_x, f) != size_x) {
			printf("data read error \n");
			exit(0);
		}

	fclose(f);
}

void write_ucmatrix(int size_x, int size_y, unsigned char** ucmatrix, char* filename) {
	int i;
	FILE* f;


	if ((fopen_s(&f, filename, "wb")) != NULL) {
		printf("%s File open Error!\n", filename);
		exit(0);

	}

	for (i = 0; i < size_y; i++)
		if (fread(ucmatrix[i], sizeof(unsigned char), size_x, f) != size_x) {
			printf("data read error \n");
			exit(0);
		}

	fclose(f);
}

int main(int argc, char* argv[]) {

	int i, j;
	IplImage* cvImg;
	CvSize imgSize;
	unsigned char** img;

	if (argc != 4) {
		printf("exe imgdata xsie ysize \n");
		exit(0);
	}

	imgSize.width = atoi(argv[2]);
	imgSize.height = atoi(argv[3]);
	img = uc_alloc(imgSize.width, imgSize.height);
	read_ucmartrix(imgSize.width, imgSize.height, img, argv[1]);

	cvImg = cvCreateImage(imgSize, 8, 1);
	for (i = 0; i < imgSize.height; i++)
		for (j = 0; j < imgSize.width; j++) {
			((unsigned char*)(cvImg->imageData + cvImg->widthStep * i))[j] = img[i][j];
		}

	cvNamedWindow(argv[1], 1);
	cvShowImage(argv[1], cvImg);

	cvWaitKey(0);

	cvDestroyWindow("image");
	cvReleaseImage(&cvImg);

	getchar();
	getchar();
	return 0;
}

```
프로그램은 위에서 설정한 opencv.cpp에서 작성한 것이며 윈도우의 cmd를 아래와 같이 실행시킨다.

자신의 opencv.exe 실행파일이 있는 곳으로 가서
<div style="width: 100%; height: 400px;">
    <img src="https://kyu9341.github.io/assets/ImageProcessing5.png" style="width: 100%
    ; height: 400px;">
</div>

위와 같이 실행파일 영상의 세로축크기 영상의 가로축 크기 순으로 입력하여 프로그램을 동작시키면 아래와 같이 이미지가 출력된다.

<div style="width: 512px; height: 512px;">
    <img src="https://kyu9341.github.io/assets/ImageProcessing6.png" style="width: 512px
    ; height: 512px;">
</div>

위와 같이 레나 영상이 잘 출력이 된 것을 확인할 수 있다.
