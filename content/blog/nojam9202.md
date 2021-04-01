---
title: "[Algorithm, C++] 백준 9202: Boggle"
date: 2021-04-01T21:58:54+09:00
slug: ""
description: ""
keywords: []
draft: true
tags: [Algorithm, C++]
math: false
toc: false
---

# 문제

상근이는 보드 게임 "Boggle"을 엄청나게 좋아한다. Boggle은 글자가 쓰여 있는 주사위로 이루어진 4×4 크기의 그리드에서 최대한 많은 단어를 찾는 게임이다.

상근이는 한 번도 부인을 Boggle로 이겨본 적이 없다. 이렇게 질 때마다 상근이는 쓰레기 버리기, 설거지와 같은 일을 해야 한다. 이제 상근이는 프로그램을 작성해서 부인을 이겨보려고 한다.

Boggle에서 단어는 인접한 글자(가로, 세로, 대각선)를 이용해서 만들 수 있다. 하지만, 한 주사위는 단어에 한 번만 사용할 수 있다. 단어는 게임 사전에 등재되어 있는 단어만 올바른 단어이다.

1글자, 2글자로 이루어진 단어는 0점, 3글자, 4글자는 1점, 5글자는 2점, 6글자는 3점, 7글자는 5점, 8글자는 11점이다. 점수는 자신이 찾은 단어에 해당하는 점수의 총 합이다.

단어 사전에 등재되어 있는 단어의 목록과 Boggle 게임 보드가 주어졌을 때, 얻을 수 있는 최대 점수, 가장 긴 단어, 찾은 단어의 수를 구하는 프로그램을 작성하시오

# 예제

```cpp
5
ICPC
ACM
CONTEST
GCPC
PROGRAMM

3
ACMA
APCA
TOGI
NEST

PCMM
RXAI
ORCN
GPCG

ICPC
GCPC
ICPC
GCPC
```

# 출력

```cpp
8 CONTEST 4
14 PROGRAMM 4
2 GCPC 2
```

# 풀이

이 문제는 Trie라는 자료구조와, DFS 문제이다.

Trie라는 자료구조는 문자열 분류에 필요한 자료구조이며, 다음과 같은 구조를 갖는다.

[https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=http%3A%2F%2Fcfile10.uf.tistory.com%2Fimage%2F996F503359E610D11E626E](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=http%3A%2F%2Fcfile10.uf.tistory.com%2Fimage%2F996F503359E610D11E626E)

이 문제에서 Boggle이라는 게임은 주어진 사전과 알파벳이 적혀있는 4x4판에서 한 칸당 하나의 문장에 사용해서 많은 단어를 찾고 점수를 얻는게 목표인 게임이다.

따라서, 사전을 Trie 자료구조를 사용해서 정리하고 DFS를 사용해서 4x4 칸에서 가능한 모든 문자열을 찾아 저장한 뒤 총 점수, 가장 긴 문자열, 문자를 찾은 갯수를 출력하면 된다.

# 코드

```cpp
#include <iostream>
#include <cstring>
#include <unordered_set>

using namespace std;

int w, b;
bool visited[4][4];
string map[4];
unordered_set<string> res;
int dy[8] = {-1, -1, -1, 0, 0, 1, 1, 1};
int dx[8] = {-1, 0, 1, -1, 1, -1, 0, 1};
int score[9] = {0, 0, 0, 1, 1, 2, 3, 5, 11};

struct Trie
{
	Trie *next[26];
	bool isFin;

	Trie()
	{
		memset(this -> next, 0, sizeof(this -> next));
		this -> isFin = false;
	}
	~Trie()
	{
		int i;

		i = 0;
		while (i < 26)
		{
			if (this -> next[i])
				delete this -> next[i];
			i++;
		}
	}

	void insert(string key)
	{
		int i, index;

		i = 0;
		Trie *pNode = this;
		while (i < key.length())
		{
			index = key[i] - 'A';
			if (!pNode -> next[index])
				pNode -> next[index] = new Trie();
			pNode = pNode -> next[index];
			i++;
		}
		pNode -> isFin = true;
	}

	void search(int y, int x, string key)
	{
		if (key.length() > 8)
			return;
		visited[y][x] = true;
		key += map[y][x];

		Trie *pNode = this -> next[map[y][x] - 'A'];
		if (pNode == NULL)
		{
			visited[y][x] = false;
			return;
		}
		if (pNode -> isFin)
		{
			res.insert(key);
		}
		for (int dir = 0; dir < 8; ++dir) {
			int ny = y + dy[dir], nx = x + dx[dir];
			if (ny < 0 || ny >= 4 || nx < 0 || nx >= 4)continue;
			if (visited[ny][nx])continue;
			pNode->search(ny, nx, key);
		}
		visited[y][x] = false;
	}
};

int main()
{
	int i, j, x, y;
	int Max, match, scoreSum;

	Trie *root = new Trie();
	cin >> w;
	i = 0;
	while (i < w)
	{
		string str;
		cin >> str;
		root -> insert(str);
		i++;
	}
	cin >> b;
	i = 0;
	j = 0;
	while (i < b)
	{
		j = 0;
		while (j < 4)
		{
			cin >> map[j];
			j++;
		}
		res.clear();
		y = 0;
		x = 0;
		while (y < 4)
		{
			x = 0;
			while (x < 4)
			{
				root -> search(y, x, "");
				x++;
			}
			y++;
		}
		string longest = "";
		Max = 0;
		match = res.size();
		scoreSum = 0;
		for (string item : res)
		{
			if (Max == item.length())
			{
				longest = longest < item ? longest : item;
			}
			else if (Max < item.length())
			{
				Max = item.length();
				longest = item;
			}
			scoreSum += score[item.length()];
		}
		cout << scoreSum << " " << longest << " " << match << '\n';
		i++;
	}
	return 0;
}
```
