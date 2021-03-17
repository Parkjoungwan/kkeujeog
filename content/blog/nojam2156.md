---
title: "[Algorithm, C++] 백준 2156 : 포도주 시식"
date: 2021-03-17T13:51:30+09:00
slug: ""
description: ""
keywords: []
draft: true
tags: [Algorithm, C++]
math: false
toc: false
---
# 문제

간략화 하자면 다음과 같다.

1. 1번째 부터 n 까지의 숫자를 더할 것이다.
2. 연속된 숫자는 2개까지 더할 수 있다.
3. 위 규칙을 지키면서 만들 수 있는 최대의 숫자를 만들어라.

# 예제

```cpp
6
6
10
13
9
8
1
```

# 출력

```cpp
33
```

# 풀이

이 문제는 이전과 같은 DP 다이나믹 프로그래밍의 일종이다.

내가 사용한 해법을 최대한 요약하면 다음과 같다.

**"각 최대값은 이전 최대값에 다음 수를 더해서 구할 수 있다. 4개 씩 끊어서 생각하자. 1 3 4로 취할 거냐 아니면 2 4로 취할 거냐. 그것도 아니면 1 4로 취할거냐."**

점화식으로 나타내면 다음과 같다.

```cpp
dp: 해당 순서에 생길 수 있는 최대값
arr: 해당 순서의 값
dp[i]는 dp[i - 3] + arr[i] + arr[i - 1] 와 dp[i - 2] + arr[i] 중 큰 것
dp[i]는 dp[i - 1]과 dp[i]를 비교해서 더 큰 것
```

# 코드

```cpp
#include <iostream>
#include <algorithm>

using namespace std;

int	main(){
	int N;
	int i;
	int dp[10001];
	int arr[10001];

	cin >> N;
	i = 1;
	dp[0] = 0;
	arr[0] = 0;
	while (i <= N){
		cin >> arr[i];
		dp [i] = 0;
		i++;
	}
	dp[1] = arr[1];
	dp[2] = arr[1] + arr[2];
	i = 3;
	while (i <= N){
		dp[i] = max(dp[i - 3] + arr[i] + arr[i - 1], dp[i - 2] + arr[i]);
		dp[i] = max(dp[i - 1], dp[i]);
		i++;
	}
	cout << dp[N];
}
```
