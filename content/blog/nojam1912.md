---
title: "[Algorithm, C++] 백준 1912 : 연속합"
date: 2021-03-14T23:52:10+09:00
slug: ""
description: ""
keywords: []
draft: true
tags: [algorithm, C++]
math: false
toc: false
---

# 문제

n개의 정수로 이루어진 임의의 수열이 주어진다. 우리는 이 중 연속된 몇 개의 수를 선택해서 구할 수 있는 합 중 가장 큰 합을 구하려고 한다. 단, 수는 한 개 이상 선택해야 한다.

예를 들어서 10, -4, 3, 1, 5, 6, -35, 12, 21, -1 이라는 수열이 주어졌다고 하자. 여기서 정답은 12+21인 33이 정답이 된다.

# 입력 조건

첫째 줄에 정수 n(1 ≤ n ≤ 100,000)이 주어지고 둘째 줄에는 n개의 정수로 이루어진 수열이 주어진다. 수는 -1,000보다 크거나 같고, 1,000보다 작거나 같은 정수이다.

# 예시

```c
10
10 -4 3 1 5 6 -35 12 21 -1
```

# 풀이 전략

연속합은 DP(단계식 프로그래밍) 알고리즘에 속하는 문제이다.

주어진 배열을 연속으로 합해서 나올 수 있는 가장 큰 값을 출력하는 문제이다.

내가 사용한 해법은

'**앞에서부터 하나씩 더 해간다고 가정할 때, 음수가 되지 않는 한 다음 수와 더 했을 때 더 큰 수 가 된다.'**

```cpp
if dp[i - 1] + dp[i] > 0
dp[i - 1] + dp[i] == 최대값이 될 수 있는 가능성이 존재.
```

# 코드

```cpp
#include <iostream>

using namespace std;

int main() {
	int N;
	int i;
	int result;
	int dp[100001];

	cin >> N;
	i = 1;
	result = -1001;
	dp[0] = 0;
	while (i <= N)
		cin >> dp[i++];
	i = 1;
	while (i <= N)
	{
		if (dp[i - 1] > 0 && dp[i] + dp[i - 1] > 0)
			dp[i] += dp[i - 1];
		if (result < dp[i])
			result = dp[i];
		i++;
	}
	cout << result;
	return 0;
}
```
