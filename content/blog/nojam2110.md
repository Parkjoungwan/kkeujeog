---
title: "[Algorithm, C++] 백준 2110 : 공유기 설치"
date: 2021-03-24T20:20:11+09:00
slug: ""
description: ""
keywords: []
draft: true
tags: [Algorithm, C++]
math: false
toc: false
---
# 문제

도현이의 집 N개가 수직선 위에 있다. 각각의 집의 좌표는 x1, ..., xN이고, 집 여러개가 같은 좌표를 가지는 일은 없다.

도현이는 언제 어디서나 와이파이를 즐기기 위해서 집에 공유기 C개를 설치하려고 한다. 최대한 많은 곳에서 와이파이를 사용하려고 하기 때문에, 한 집에는 공유기를 하나만 설치할 수 있고, 가장 인접한 두 공유기 사이의 거리를 가능한 크게 하여 설치하려고 한다.

C개의 공유기를 N개의 집에 적당히 설치해서, 가장 인접한 두 공유기 사이의 거리를 최대로 하는 프로그램을 작성하시오.

# 예제

```cpp
5 3
1
2
8
4
9
```

# 출력

```cpp
3
```

# 풀이

이진탐색 문제이다.

풀이 방법은 다음과 같다.

"특정 간격을 기준으로 공유기가 설치됐을 때, 공유기를 모두 설치했는가 아닌가, 더 설치해야된다면 거리를 줄이고 공유기 수를 줄여야한다면 거리를 늘린다."

# 코드

```cpp
#include <iostream>
#include <algorithm>
using namespace std;

int main()
{
	int N;
	int C;
	int i;
	int *arr;

	cin >> N;
	cin >> C;
	arr = new int[N];
	i = 0;
	while (i < N)
	{
		cin >> arr[i];
		i++;
	}
	sort(arr, arr + N);

	int start, end, mid;
	int d;
	int ans, count, target;

	start = 1;
	end = arr[N - 1] - arr[0];
	d = 0;
	count = 1;
	while (start <= end)
	{
		i = 1;
		mid = (start + end) / 2;
		count = 1;
		target = arr[0];
		while (i < N)
		{
			d = arr[i] - target;
			if (mid <= d)
			{
				count++;
				target = arr[i];
			}
			i++;
		}
		if (count >= C)
		{
			ans = mid;
			start = mid + 1;
		}
		else
		{
			end = mid - 1;
		}
	}
	cout << ans;
}
```
