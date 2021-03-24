---
title: "[Algorithm, C++] 백준 1325 : 효율적인 해킹"
date: 2021-03-24T20:10:46+09:00
slug: ""
description: ""
keywords: []
draft: true
tags: [Algorithm, C++]
math: false
toc: false
---
# 문제

해커 김지민은 잘 알려진 어느 회사를 해킹하려고 한다. 이 회사는 N개의 컴퓨터로 이루어져 있다. 김지민은 귀찮기 때문에, 한 번의 해킹으로 여러 개의 컴퓨터를 해킹 할 수 있는 컴퓨터를 해킹하려고 한다.

이 회사의 컴퓨터는 신뢰하는 관계와, 신뢰하지 않는 관계로 이루어져 있는데, A가 B를 신뢰하는 경우에는 B를 해킹하면, A도 해킹할 수 있다는 소리다.

이 회사의 컴퓨터의 신뢰하는 관계가 주어졌을 때, 한 번에 가장 많은 컴퓨터를 해킹할 수 있는 컴퓨터의 번호를 출력하는 프로그램을 작성하시오.

# 예제

```cpp
5 4
3 1
3 2
4 3
5 3
```

# 출력

```cpp
1 2
```

# 풀이

이 문제는 그래프 탐색 중에 하나인 "DFS" 깊이 우선 탐색 문제이다.

내가 사용한 방법은 단순하다.

"깊이 우선탐색으로 제일 멀리간 노드들을 출력한다."

# 코드

```cpp
#include <iostream>
#include <algorithm>
#include <vector>
#include <cstring>
using namespace std;

int vis[10001];
vector<int> com[10001];
int Num;

void DFS(int x)
{
	int i;
	int next_x;

	vis[x] = 1;
	i = 0;
	while(i < com[x].size())
	{
		next_x = com[x][i];
		if (vis[next_x] != 1)
		{
			Num++;
			DFS(next_x);
		}
		i++;
	}
}

int main()
{
	int N;
	int M;
	int i;
	int a;
	int b;
	int tmp_ans;
	vector<int> ans;

	cin >> N;
	cin >> M;
	i = 0;
	tmp_ans = -1;
	while (i < M)
	{
		cin >> a;
		cin >> b;
		com[b].push_back(a);
		i++;
	}
	i = 1;
	while (i <= N)
	{
		memset(vis, 0, sizeof(vis));
		Num = 0;
		DFS(i);

		if (tmp_ans == Num)
			ans.push_back(i);
		else if (tmp_ans < Num)
		{
			tmp_ans = Num;
			ans.clear();
			ans.push_back(i);
		}
		i++;
	}
	sort(ans.begin(), ans.end());
	i = 0;
	while (i < ans.size())
	{
		cout << ans[i] << " ";
		i++;
	}
}
```
