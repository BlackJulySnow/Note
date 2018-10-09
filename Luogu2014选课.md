---
title: Luogu2014
tags: DP,树形DP,背包
grammar_cjkRuby: true
---


``` cpp
#include<cstdio>
#include<cstring>
#include<vector>
#define MAXN 305
#define max(a,b) (a > b ? a : b)
using namespace std;
int read(){
	char c = getchar();
	while(c < '0' || '9' < c)
		c = getchar();
	int x = 0;
	while('0' <= c && c <= '9'){
		x = 10 * x + c - '0';
		c = getchar();
	}
	return x;
}

int n,m,s[MAXN],f[MAXN][MAXN],tmp;
vector<int> son[MAXN];
void dp(int x){
	f[x][0] = 0;//x节点不选的学分值为0
	if(son[x].empty()){
		f[x][1] = s[x];//树叶选一门时候默认是自己，所以获得自己的学分
		return;
	}
	for(unsigned int i = 0;i < son[x].size();i++){
		int y = son[x][i];
		dp(y);
		for(int t = m;t >= 0;t--){
			for(int j = t;j >= 0;j--){
				if(t >= j){
					f[x][t] = max(f[x][t],f[x][t - j] + f[y][j]);//表示x是否选其字节点有k门课程的最大值
				}
			}
		}
	}
	for(int t = m;t > 0;t--)
		f[x][t] = f[x][t - 1] + s[x];//其默认要占一个坑位
}
int main(){
	//freopen("choose0.in","r",stdin);
	//freopen("A.out","w",stdout);
	n = read();
	m = read();
	for(int i = 1;i <= n;i++){
		tmp = read();
		s[i] = read();//学分
		son[tmp].push_back(i);
	}
	memset(f,~0x3f,sizeof(f));
	f[0][0] = 0;
	for(unsigned int i = 0;i < son[0].size();i++){
		int y = son[0][i];
		dp(y);
		for(int j = m;j >= 0;j--){
			for(int k = j;k >= 0;k--){
				if(j >= k){
					f[0][j] = max(f[0][j],f[0][j - k] + f[y][k]);
				}
			}
		}
	}
	printf("%d\n",f[0][m]);
	return 0;
}
```