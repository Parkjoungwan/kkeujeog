---
title: "[Algorithm, C++] 백준 1300 : k번째 수"
date: 2021-03-24T17:58:20+09:00
slug: ""
description: ""
keywords: []
draft: true
tags: [Algorithm, C++]
math: false
toc: false
---

# 문제

세준이는 크기가 N×N인 배열 A를 만들었다. 배열에 들어있는 수 A[i][j] = i×j 이다. 이 수를 일차원 배열 B에 넣으면 B의 크기는 N×N이 된다. B를 오름차순 정렬했을 때, B[k]를 구해보자.

배열 A와 B의 인덱스는 1부터 시작한다.

# 예제

```cpp
3
7
```

# 출력

```cpp
6
```

# 풀이

문제에서 나오는 배열을 만들었다가는 메모리 초과로 실패를 겪게 된다.
이진탐색을 사용해야 하는데, 막상 이진탐색 문제라는게 분류에 적혀있어도 어떻게 적용해야 할 지 막막하다.

결론부터 말하면 다음과 같다.

1. N x N 행렬을 정렬한 배열에서 K번째 값은 K보다 작거나 같다.
2. N x N 행렬을 정렬한 배열의 K번째 값이라는 건, K보다 작은 값이 K개 존재한다는 뜻이다.
3. 따라서 1 ~ K까지의 값 중에 우리가 원하는 순서 K번째 배열의 값이 존재하고, 그 값보다 작은 값이 K개 존재한다.
4. 이진탐색으로 1과 K의 중간 값부터, 이진탐색으로 탐색을 하면서 해당값의 이전의 값들을 세줘야 한다.
5. 그 값들의 수를 세주기 위해서 사용한 점화식은 "min(mid / i, N)" 이다.

# 코드

```cpp
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;

int		main()
{
	int N;
	int k, i;
	int start, end, mid, target;
	int	result;

	cin >> N;
	cin >> k;
	result = 0;
	start = 1;
	end = k;
	while (start <= end)
	{
		mid = (start + end) / 2;
		target = 0;
		i = 1;
		while (i <= N)
		{
			target += min(mid / i, N);
			i++;
		}
		if (target < k)
			start = mid + 1;
		else
		{
			result = mid;
			end = mid - 1;
		}
	}
	cout << result;
	return 0;
}
```
