# 💚 1026. 点对

**单点时限:** 1.0 sec

**内存限制:** 512 MB

直线上有$$n$$ 个点 ($$n$$ 为偶数) : $$L=x_1,x_2,⋯,x_n$$，把$$n$$ 个点分成$$\frac{n}{2}$$ 组点对 $$(a_i,b_i)$$，$$i=1,2,⋯,\frac{n}{2}$$ ，$$a_i,b_i∈L$$，使得 $$∑_{i=1}^{\frac{n}{2}}|a_i−b_i|$$ 最小。

请编写程序计算该最小值。

### 输入格式

输入数据第一行包含一个整数$$n$$ ，表示点的数量。

第二行包含 $$n$$ 个用空格隔开的整数$$x_1,x_2,⋯,xn$$。

### 输出格式

输出包含一个整数，表示$$∑_{i=1}^{\frac{n}{2}}|a_i−b_i|$$的最小值。

### 样例

{% tabs %}
{% tab title="样例1" %}
{% code title="input" %}
```
4
1 2 3 4
```
{% endcode %}

{% code title="output" %}
```
2
```
{% endcode %}
{% endtab %}

{% tab title="样例2" %}
{% code title="input" %}
```
4
0 0 0 0
```
{% endcode %}

{% code title="output" %}
```
0
```
{% endcode %}


{% endtab %}
{% endtabs %}

{% hint style="info" %}
**数据分布**

20分任务，$$1≤n≤10$$， $$−10^4$$≤$$x_i$$≤$$10^4$$

20分任务，$$1≤n≤1000$$，$$−10^4≤x_i≤10^4$$

30分任务，$$1≤n≤10^5$$，$$−10^4≤x_i≤10^4$$

30分任务，$$1≤n≤10^5$$，$$−10^9≤x_i≤10^9$$
{% endhint %}

### 题解

{% code lineNumbers="true" %}
```c
#include <stdio.h> 
#include <stdlib.h>

typedef long long int lli;

lli llabs(lli x){ return x < 0 ? -x : x;}

int cmp(const void *a, const void *b)
{
	lli na = *(lli *)a;
	lli nb = *(lli *)b;
	return na > nb ? 1 : -1 ;
}

int main(void)
{
	int n;
	scanf("%d", &n);
	lli point[n], distance = 0;
	for(int i = 0 ; i < n ; i ++){
		scanf("%lld", &point[i]);
	}
	qsort(point, n, sizeof(lli), cmp);
	for(int i = 0 ; i < n ; i += 2){
		distance += llabs(point[i] - point[i + 1]);
	}
	printf("%lld", distance);
	
	return 0;
}
```
{% endcode %}
