# 💙 1031. 最小向量点积

**单点时限:** 2.0 sec

**内存限制:** 256 MB

两个向量$$a=[a1,a2,⋯,an]$$和$$b=[b1,b2,⋯,bn]$$ 的点积定义为 :

![](https://acm.ecnu.edu.cn/upload/3003/3003s1.png)

例如，两个三维向量$$[1,3,−5]$$ 和 $$[4,−2,−1]$$的点积是

![](https://acm.ecnu.edu.cn/upload/3003/3003s2.png)

假设允许对每个向量中的坐标值进行重新排列。找出所有排列中点积最小的一种排列，输出最小的那个点积值。

上例中的一种排列$$[3,1,−5]$$ 和$$[−2,−1,4]$$ 的点积为$$−27$$，这是最小的点积。

### 输入格式

第$$1$$ 行：一个整数 $$T (1⩽T⩽10)$$为问题数。

接下来每个问题有 $$3$$ 行。第$$1$$ 行是一个整数 $$n (1⩽n⩽1000)$$，表示两个向量的维数。第$$2$$行和第$$3$$ 行分别表示向量$$a$$ 和向量 $$b$$。每个向量都有$$n$$ 个由一个空格分隔的坐标值 坐标值$$(−1000⩽坐标值⩽1000)$$ 组成。

### 输出格式

对于每个问题，输出一行问题的编号（`0` 开始编号，格式：`case #0:` 等）。

然后对应每个问题在一行中输出最小点积值。

### 样例

{% tabs %}
{% tab title=" 样例1" %}
{% code title="input" %}
```
3
3
3 1 -5
-2 -1 4
1
2
-298
5
1 2 3 4 5
1 0 1 0 1
```
{% endcode %}

{% code title="output" %}
```
case #0:
-27
case #1:
-596
case #2:
6
```
{% endcode %}
{% endtab %}
{% endtabs %}

### 题解

{% code lineNumbers="true" %}
```c
#include <stdio.h>
#include <stdlib.h>

typedef long long int lli;

int ascand(const void *a, const void *b)
{
	lli na = *(lli *)a;
	lli nb = *(lli *)b;
	return na > nb ? 1 : -1;
}

int descand(const void *a, const void *b)
{
	lli na = *(lli *)a;
	lli nb = *(lli *)b;
	return na < nb ? 1 : -1;
}

int main(void)
{
	int t;
	scanf("%d", &t);
	for(int i = 0 ; i < t; i ++){
		int n;
		lli product = 0;
		scanf("%d", &n);
		lli vector1[n], vector2[n];
		for(int j = 0 ; j < n ; j ++){
			scanf("%lld", &vector1[j]);
		}
		for(int j = 0 ; j < n ; j ++){
			scanf("%lld", &vector2[j]);
		}
		qsort(vector1, n, sizeof(lli), ascand);
		qsort(vector2, n, sizeof(lli), descand);
		for(int j = 0 ; j < n ; j ++){
			product += (vector1[j] * vector2[j]);
		}
		printf("case #%d:\n", i);
		printf("%lld\n", product);
	}
}
```
{% endcode %}
