---
layout: post
title: "버블정렬(Bubble Sort) 알고리즘"
subtitle: "Baekjoon algorithm"
date: 2019-12-21 17:31:15
author: kwon
categories: algorithm
---
저번 포스팅에서는 선택정렬에 대해 공부해보았고 이번에는 버블 정렬에 대해 알아보겠다. 마찬가지로 일련의 숫자들을 오름차순으로 정렬하는 문제이다.

## 버블 정렬
>거품 정렬(Bubble sort)은 두 인접한 원소를 검사하여 정렬하는 방법이다. 시간 복잡도가 O(n^2)로 상당히 느리지만, 코드가 단순하기 때문에 자주 사용된다. 원소의 이동이 거품이 수면으로 올라오는 듯한 모습을 보이기 때문에 지어진 이름이다.
출처 - 위키백과


버블 정렬 또한 선택 정렬과 같이 아주 직관적인 해결방법인데 바로 가까이에 있는 두 숫자끼리 비교를 하여 더 작은 숫자를 앞으로 보내주는 것을 반복하는 것이다.

```C++
#include<iostream>

using namespace std;

int main()
{
	int i, j, temp;
	int array[10] = { 1, 10, 5, 8, 7, 6, 4, 3, 2, 9 };
	//int array[10] = { 10, 9, 8, 7, 6, 5, 4, 3, 2, 1 };

	for (i = 0; i < 10; i++)
	{
		for (j = 0; j < 9 - i; j++) // 버블정렬은 뒤쪽부터 정렬이 되므로 9-i만큼만 반복하면 된다.
		{
			if (array[j] > array[j + 1])
			{
				temp = array[j];
				array[j] = array[j + 1];
				array[j + 1] = temp;
			}
		}
		for (j = 0; j < 10; j++)
		{
			cout << array[j] << " ";
		}
		cout << "\n";
	}

	cout << "결과 :";
	for (i = 0; i < 10; i++)
	{
		cout << array[i] << " ";
	}

	return 0;
}
```
위와 같이 코드로 표현할 수 있다.

<div style="width: 200px; height: 200px;">
    <img src="https://kyu9341.github.io/assets/bubblesort.png" style="width: 200px
    ; height: 200px;">
</div>

참조 : <https://blog.naver.com/ndb796/221226803544>