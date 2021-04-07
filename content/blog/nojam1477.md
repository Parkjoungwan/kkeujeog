---
title: "[Algorithm, C++] 백준 1477: 휴게소 세우기"
date: 2021-04-07T16:48:53+09:00
slug: ""
description: ""
keywords: []
draft: true
tags: [Algorithm, C++]
math: false
toc: false
---
# 문제

다솜이는 유료 고속도로를 가지고 있다. 다솜이는 현재 고속도로에 휴게소를 N개 가지고 있는데, 휴게소의 위치는 고속도로의 시작으로부터 얼만큼 떨어져 있는지로 주어진다. 다솜이는 지금 휴게소를 M개 더 세우려고 한다.

다솜이는 이미 휴게소가 있는 곳에 휴게소를 또 세울 수 없고, 고속도로의 끝에도 휴게소를 세울 수 없다. 휴게소는 정수 위치에만 세울 수 있다.

다솜이는 이 고속도로를 이용할 때, 모든 휴게소를 방문한다. 다솜이는 휴게소를 M개 더 지어서 휴게소가 없는 구간의 길이의 최댓값을 최소로 하려고 한다. (반드시 M개를 모두 지어야 한다.)

예를 들어, 고속도로의 길이가 1000이고, 현재 휴게소가 {200, 701, 800}에 있고, 휴게소를 1개 더 세우려고 한다고 해보자.

일단, 지금 이 고속도로를 타고 달릴 때, 휴게소가 없는 구간의 최댓값은 200~701구간인 501이다. 하지만, 새로운 휴게소를 451구간에 짓게 되면, 최대가 251이 되어서 최소가 된다.

# 예제

```cpp
6 7 800
622 411 201 555 755 82
```

# 출력

```cpp
70
```

# 풀이

"우리가 찾는건 최대값의 최소인 경우이다."

말 그대로, 최대가 아닌 값들에 대해서 신경 쓸 필요가 없다.

따라서, mid를 우리가 찾는 최대라고 가정하고 각 휴게소의 거리를 나눠보면서 우리가 입력값으로 받아온 m과 일치하는지 확인하면서 이분탐색을 실시한다.

이 과정으로 start가 end보다 커질 때 까지 반복하면, start가 우리가 찾는 가장 작은 최대값이 된다.

# 코드

```cpp
#include <iostream>
#include <algorithm>
#include <vector>
#include <string.h>
using namespace std;

int main()
{
	int n, m, l;
	int pos;
	int i;
	int start, end, mid;

	cin >> n >> m >> l;
	vector<int>store;
	store.push_back(0);
	i = 0;
	while (i < n)
	{
		cin >> pos;
		store.push_back(pos);
		i++;
	}
	store.push_back(l);
	sort(store.begin(), store.end());
	start = 1;
	end = l - 1;
	mid = 0;
	while (start <= end)
	{
		mid = (start + end) / 2;
		int new_store = 0;
		i = 1;
		while (i < store.size())
		{
			int dist = store[i] - store[i - 1];
			new_store += (dist / mid);
			if (dist % mid == 0)
				new_store--;
			i++;
		}
		if (new_store > m)
			start = mid + 1;
		else
			end = mid - 1;
	}
	cout << start;
	return 0;
}
```
