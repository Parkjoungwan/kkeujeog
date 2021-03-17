---
title: "[Algorithm, C++] 백준 2193 : 이친수"
date: 2021-03-17T16:51:45+09:00
slug: ""
description: ""
keywords: []
draft: true
tags: [Algorithm, C++]
math: false
toc: false
---

# 문제

간략하게 나타내면 다음과 같다.

이친수란, 다음의 규칙을 만족하는 이진수이다.

1. 이친수는 0으로 시작하지 않는다.
2. 이친수는 1이 연속 두번으로 올 수 없다.

입력값 N이 주어졌을때, N자리 수의 이친수 개수를 구하자.

# 예제

```cpp
3
```

# 출력

```cpp
2
```

# 풀이

이 문제도 이전과 같은 DP 다이나믹 프로그래밍 문제이다.

내가 사용한 해법은 요약하면 다음과 같다.

**"마지막 숫자가 0으로 끝나는 이친수는 0과 1을 붙일 수 있고, 마지막 숫자가 1로 끝나는 이친수는 0을 붙일 수 있다."**

점화식으로 나타내면 다음과 같다.

```cpp
dpzero[i] = dpzero[i - 1] + dpone[i - 1];
dpone[i] = dpzero[i- 1];
dp[i] = dpzero + dpone;
dpzero + dpone 은 이친수의 개수
```

단, 이 문제는 90까지의 경우를 생각하고 INT값을 초과하는 값을 가질 수도 있기 때문에 변수 자료형에 신경써줘야한다.

# 코드

```cpp
#include <iostream>

using namespace std;

int main()
{
	int N;
	int i;
	long long dpzero[91];
	long long dpone[91];

	cin >> N;
	dpzero[0] = 0;
	dpone[0] = 0;
	dpzero[1] = 0;
	dpone[1] = 1;
	i = 2;
	while (i <= N)
	{
		dpzero[i] = dpzero[i - 1] + dpone[i - 1];
		dpone[i] = dpzero[i - 1];
		i++;
	}
	cout << dpzero[N] + dpone[N];
	return 0;
```
