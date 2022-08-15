# 💛 1024. 成绩排序

**单点时限:** 2.0 sec

**内存限制:** 256 MB

C 语言课程的上机考试有 M 道题目，从第 1 题至第 M 题每道题的分值为 均为大于的正整数$$a_1,a_2,⋯,a_m,(a_1,a_2,⋯,a_m均为大于0的正整数)$$

现给定分数线，请编写程序输出不低于该分数线的学生人数，并将他 (她) 们的成绩按降序输出，若有多名学生考试成绩相同，则按他 (她) 们学号的升序输出。

### 输入格式

第 1 行：一个整数$$T (1≤T≤10)$$ 为问题数。

对于每组测试数据：

第 1 行是三个正整数：学生人数$$N(0<N<500)$$、考试题目数$$M(0<M≤10)$$、分数线 $$G(G>0)$$

第 2 行有 $$M$$ 个正整数：分别为第 $$1$$ 题至第$$M$$ 题的分值$$a_1,a_2，⋯,a_m(0<a_1,a_2,⋯,a_m≤10^8)$$；

接下来$$N$$ 行，每行给出一名学生的学号（长度为$$11$$ 的字符串，学号的第一个字符可能为$$0$$）、该学生解出的题目总数 $$S(S≥0)$$、以及这$$S$$ 道题的题号（题号由$$1$$ 到$$M$$）。

### 输出格式

对于每个问题，输出一行问题的编号（`0`开始编号，格式：`case #0:` 等）

然后首先在一行中输出不低于分数线的学生人数，接下来每行按考试成绩从高到低输出上分数线的学生的学号与成绩，其间用 1 个空格分隔。若有多名学生成绩相同，则按他 (她) 们学号的升序输出。

### 样例

{% tabs %}
{% tab title="样例1" %}
{% code title="input" %}
```
3
4 5 25
10 10 12 13 15
10091130015 3 5 1 3
10091130013 5 2 4 1 3 5
10091130012 2 1 2
10091130011 3 2 3 5
2 3 20
10 10 10
10101130012 0
10101130019 2 1 2
1 2 40
10 30
10101130018 1 2
```
{% endcode %}

{% code title="output" %}
```
case #0:
3
10091130013 60
10091130011 37
10091130015 37
case #1:
1
10101130019 20
case #2:
0
```
{% endcode %}
{% endtab %}
{% endtabs %}

### 题解

{% code lineNumbers="true" %}
```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

typedef struct{
	char id[12];
	int solved;
	int total;
}Info;

int cmp(const void *a, const void *b)
{
	Info * pa = (Info *)a;
	Info * pb = (Info *)b;
	if(pa->total != pb->total){
		return pb->total - pa->total;
	}else{
		return strcmp(pa->id, pb->id);
	}
}

int main(void)
{
	int t;
	scanf("%d", &t);
	for(int i = 0 ; i < t; i ++){
		int NumOfStu, NumOfQues, line;
		scanf("%d %d %d", &NumOfStu, &NumOfQues, &line);
		int scores[NumOfQues], temp, cnt = 0;
		Info stu[NumOfStu];
		for(int j = 0 ; j < NumOfQues; j ++){
			scanf("%d", &scores[j]);
		}
		for(int j = 0 ; j < NumOfStu; j ++){
			scanf("%s %d", stu[j].id, &stu[j].solved);
			stu[j].total = 0;
			for(int k = 0 ; k < stu[j].solved; k ++){
				scanf("%d", &temp);
				stu[j].total += scores[temp - 1];
			}
		}
		qsort(stu, NumOfStu, sizeof(Info), cmp);
		for(; stu[cnt].total >= line; cnt ++);
		printf("case #%d:\n", i);
		printf("%d\n", cnt);
		for(int j = 0 ; j < NumOfStu; j ++){
			if(stu[j].total >= line){
				printf("%s %d\n", stu[j].id, stu[j].total);
			}
		}
	}
	
	return 0;
}
```
{% endcode %}
