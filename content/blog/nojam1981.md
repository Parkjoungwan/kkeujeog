---
title: "[Algorithm, C++] 백준 1981: 배열의 이동"
date: 2021-04-11T17:35:25+09:00
slug: ""
description: ""
keywords: []
draft: true
tags: [Algorithm, C++]
math: false
toc: false
---
# 문제

n×n짜리의 배열이 하나 있다. 이 배열의 (1, 1)에서 (n, n)까지 이동하려고 한다. 이동할 때는 상, 하, 좌, 우의 네 인접한 칸으로만 이동할 수 있다.

이와 같이 이동하다 보면, 배열에서 몇 개의 수를 거쳐서 이동하게 된다. 이동하기 위해 거쳐 간 수들 중 최댓값과 최솟값의 차이가 가장 작아지는 경우를 구하는 프로그램을 작성하시오.

# 예제

```cpp
5
1 1 3 6 8
1 2 2 5 5
4 4 0 3 3
8 0 2 3 4
4 3 0 2 1
```

# 출력

```cpp
2
```

# 풀이

이 문제도 이분탐색 문제다.

"배열의 값을 입력받으면서 end, start 값을 찾고 이분탐색을 진행한다."

그런데, 이게 가능한지 가능하지 않은지에 대해서 판단하는 기준은,

"BFS로 처음부터 끝까지 진행하면서, 가능하면 end를 줄이고 불가능하면 start를 늘린다."

이를 반복해서 start와 end가 같아질 때가지 반복하면, 그 값이 차이 값의 최소값이다.

# 코드

```cpp
#include <iostream>
#include <algorithm>
#include <vector>
#include <cmath>
#include <queue>
#include <cstring>

using namespace std;
int N, Max, Min;
int arr[100][100];
bool visit[100][100];
int i, j, k, l;
int dx[] = {0, 0, 1, -1};
int dy[] = {1, -1, 0, 0};

bool fd_bfs(int mid)
{
	queue< pair<int, int> > que;
	i = Min;
	while (i < Max)
	{
		memset(visit, true, sizeof(visit));
		j = 0;
		while (j < N)
		{
			k = 0;
			while (k < N)
			{
				if (i <= arr[j][k] && arr[j][k] <= i + mid)
					visit[j][k] = false;
				k++;
			}
			j++;
		}
		que.push(make_pair(0,0));
		while (!que.empty())
		{
			int x = que.front().first;
			int y = que.front().second;
			que.pop();

			if (visit[x][y] == true)
				continue;
			visit[x][y] = true;
			if (x == N - 1 && y == N - 1)
				return true;
			l = 0;
			while (l < 4)
			{
				int nx = x + dx[l];
				int ny = y + dy[l];

				if (nx >= 0 && ny >= 0 && nx < N && ny < N)
					que.push(make_pair(nx, ny));
				l++;
			}
		}
		i++;
	}
	return false;
}

int main()
{
	ios::sync_with_stdio(false);
	cin.tie(NULL);
	cout.tie(NULL);

	int start, mid, end;

	Max = -1;
	Min = 201;
	cin >> N;
	i = 0;
	while (i < N)
	{
		j = 0;
		while (j < N)
		{
			cin >> arr[i][j];
			if (arr[i][j] > Max)
				Max = arr[i][j];
			if (arr[i][j] < Min)
				Min = arr[i][j];
			j++;
		}
		i++;
	}
	start = 0;
	end = Max - Min;
	while (start <= end)
	{
		mid = (start + end) / 2;
		if (fd_bfs(mid))
			end = mid - 1;
		else
			start = mid + 1;
	}
	cout << end + 1;
}
```
