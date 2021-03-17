---
title: "[Algorithm, C++] 백준 6359 : 만취한 상범"
date: 2021-03-17T16:59:05+09:00
slug: ""
description: ""
keywords: []
draft: true
tags: [Algorithm, C++]
math: false
toc: false
---
# 문제

테스트 케이스 T, 정수 N을 입력받는다.

N개의 감옥은 모두 잠겨있다.

그리고 1부터 N이 될 때까지 1씩 증가하며,

해당 값의 곱셈 값의 감옥문을 잠겼으면 열고 열렸으면 잠근다.

열려있는 감옥의 수를 구하라.

# 예제

```cpp
2
5
100
```

# 출력

```cpp
2
10
```

# 풀이

단순 구현문제로 볼 수 있는 문제다.

모든 감옥을 배열로 구현하고 위에서 시키는대로 열고 닫아도 시간 초과가 나지않는 걸로 보인다.

구현하다가 알게된 해법은 다음과 같다.

**"N과 같거나 작은 제곱수의 감옥의 문이 열려있다."**

1부터 증가하면 곱셈의 문을 열거나 닫게되면 각 값의 공배수들은 닫히거나 열리거나를 짝수번 반복하게 된다.

그런데 제곱수는 홀수번 반복하게되어,  열려있게 된다. 

# 코드

```cpp
#include <iostream>

using namespace std;

int main()
{
	int T;
	int N;
	int i;
	int j;
	int result;

	cin >> T;
	i = 1;
	while (i <= T)
	{
		cin >> N;
		j = 1;
		while (j <= N)
		{
			if (j * j <= N)
				result++;
			j++;
		}
		j = 0;
		cout << result << '\n';
		result = 0;
		i++;
	}
	return 0;
}
```
