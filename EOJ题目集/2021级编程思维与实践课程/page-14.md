# 💛 1019. 发愁

**单点时限:** 2.0 sec

**内存限制:** 256 MB

大家来到小强家，发现小强正发愁，原来小强正为 “ACM 足球超级联赛 ” 的排名而发愁。

既然过生日，就应该开开心心的，所以作为超级程序员的你应当挺身而出！

所以请你，天才程序员帮下忙，写个程序根据比赛情况计算出各队排名。

### 输入格式

有多组测试数据。

每组数据先输入两个整数 $$n (0≤n≤10)$$和 $$m (0≤m≤2∗n∗n)$$，$$n$$ 代表球队数量，$$m$$ 代表比赛场数。

接下来 m 行，每行有三个数  $$a,b,c$$。 $$a,b (1≤a,b≤n)$$表示球队的编号。

$$c=1$$ ，表示 ‘a 胜 b’；

$$c=−1$$ ，表示 ‘b 胜 a’；

$$c=0$$ ，表示 ‘a、b 战成平局’。

胜者球队积分加 $$3$$ 分，负者球队积分扣$$1$$ 分，平局双方各加$$1$$ 分。

（输入不会有自己打自己的情况，两个队之间可能有多场比赛）

$$n=m=0$$ 表示输入结束，不用处理这组数据。

每个球队的初始积分为0。

### 输出格式

在一行中按照排名输出各队的编号，每个数的后面输出一个空格，最后一个数后面没有空格。

排名规则：

1: 积分高的队排前面。

2: 积分一样的队胜场数多的排前面。

3: 积分一样且胜场数一样的队负场数少的排前面。

4: 若还不能分出先后，编号小的排前面。

### 样例

{% tabs %}
{% tab title="样例1" %}
{% code title="input" %}
```
4 4
1 2 1
2 1 1
3 4 1
3 4 -1
4 1
1 3 -1
4 1
3 2 0
4 4
4 1 1
3 2 0
3 2 0
3 1 0
4 2
3 1 1
1 4 0
0 0
```
{% endcode %}

{% code title="output" %}
```
1 2 3 4
3 2 4 1
2 3 1 4
4 3 2 1
3 4 2 1
```
{% endcode %}


{% endtab %}
{% endtabs %}

### 题解

{% code lineNumbers="true" %}
```c
#include <stdio.h>
#include <stdlib.h>


typedef struct{
	int id;
	int score;
	int win;
	int lose;
}Info;

int cmp(const void *a, const void *b)
{
	Info *pa = (Info *)a;
	Info *pb = (Info *)b;	
	if(pa->score != pb->score){
		return pa->score > pb->score ? -1: 1;
	}else if(pa->win != pb->win){
		return pa->win > pb->win ? -1 : 1;
	}else if(pa->lose != pb->lose){
		return pa->lose > pb->lose ? 1 : -1;
	}else{
		return pa->id - pb->id;
	}
}

int main(void)
{
	int n, m;
	int t1, t2, c;
	scanf("%d %d", &n, &m);
	while(n != 0 || m != 0){
		Info team[n];
		for(int i = 0 ; i < n ; i ++){
			team[i].id = i + 1;
			team[i].score = team[i].win = team[i].lose = 0;
		}
		for(int i = 0; i < m ; i ++){
			scanf("%d %d %d", &t1, &t2, &c);
			if(c == 1){
				team[t1 - 1].score += 3;
				team[t1 - 1].win ++;
				team[t2 - 1].score -= 1;
				team[t2 - 1].lose ++;
			}else if(c == 0){
				team[t1 - 1].score += 1;
				team[t2 - 1].score += 1;
			}else if(c == -1){
				team[t2 - 1].score += 3;
				team[t2 - 1].win ++;
				team[t1 - 1].score -= 1;
				team[t1 - 1].lose ++;
			}
		}
		qsort(team, n, sizeof(Info), cmp);
		for(int i = 0 ; i < n ; i ++){
			printf("%d%c", team[i].id, i == n - 1?'\n':' ');
		}
		scanf("%d %d", &n, &m);
	}
	
	return 0;
}
```
{% endcode %}
