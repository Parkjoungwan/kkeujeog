---
title: "[Algorithm, C++] 백준 2022: 사다리"
date: 2021-04-07T17:38:24+09:00
slug: ""
description: ""
keywords: []
draft: true
tags: [Algorithm, C++]
math: false
toc: false
---

# 문제

아래의 그림과 같이 높은 빌딩 사이를 따라 좁은 길이 나있다. 두 개의 사다리가 있는데 길이가 x인 사다리는 오른쪽 빌딩의 아래를 받침대로 하여 왼쪽 빌딩에 기대져 있고 길이가 y인 사다리는 왼쪽 빌딩의 아래를 받침대로 하여 오른쪽 빌딩에 기대져 있다. 그리고 두 사다리는 땅에서부터 정확하게 c인 지점에서 서로 교차한다. 그렇다면 두 빌딩은 얼마나 떨어져 있는 걸까?

![https://onlinejudgeimages.s3-ap-northeast-1.amazonaws.com/upload/201007/ladd.png](https://onlinejudgeimages.s3-ap-northeast-1.amazonaws.com/upload/201007/ladd.png)

# 예제

```cpp
30 40 10
```

# 출력

```cpp
26.033
```

# 풀이

"주어진 값들로 수식을 세워야 한다."

찾아야 하는 아래쪽, 두 집 사이의 거리 w

첫번째 집에 걸쳐져 있는 사다리의 높이 h1

두번째 집에 걸쳐져 있는 사다리의 높이 h2

두 사다리의 교점에서 수직으로 내린 길이 c
두 사다리의 교점에서 수직으로 내렸을 때 나눠지는 두 길이 w_1, w_2

수식을 세웠다면, 0부터 x와 y값 중 작은 값을 기준으로 이분탐색으로 w를 찾아나가면 된다.

```markdown
w = w_1 + w_2

h1 / w = c / w_2

w_2 = cw / h1

h2 / w = c / w_1

w_1 = cw / h2

w = w * c / h2 + w * c /  h1

1 = c (h1 + h2) / (h1 * h2)

"c = (h1 + h2) / (h1 * h2)"
```

# 코드

```cpp
#include <iostream>
#include <algorithm>
#include <vector>
#include <cmath>

using namespace std;

double x, y, c;

double fd_width(double mid)
{
	double h1 = sqrt(x * x - mid * mid);
	double h2 = sqrt(y * y - mid * mid);
	return (h1 * h2)/(h1 + h2);
}

int main()
{
	ios::sync_with_stdio(false);
	cin.tie(NULL);
	cout.tie(NULL);

	cin >> x >> y >> c;
	double start, end, mid;
	double result;

	start = 0;
	end = min(x, y);

	while (end - start > 0.000001 )
	{
		mid = (start + end) / 2.0;
		if (fd_width(mid) >= c)
		{
			result = mid;
			start = mid;
		}
		else
			end = mid;
	}
	printf("%.3lf\n", result);
	return 0;
}
```
