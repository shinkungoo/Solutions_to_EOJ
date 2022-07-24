# 1013. 平衡三进制

**单点时限:** 2.0 sec

**内存限制:** 512 MB

平衡三进制记数系统以 $$3$$ 为基数，但其数码不是使用数字 $$0$$、$$1$$ 和 $$2$$ ，而是用数字$$−1$$、$$0$$ 和 $$1$$ 来表示一个数码。

下表给出平衡三进制数对应的十进制数，其中我们以 2 表示 −1。

|      平衡三进制      |         十进制        |
| :-------------: | :----------------: |
| ****$$102$$**** |        $$8$$       |
|   $$1120.22$$   |  $$32\frac{5}{9}$$ |
|   $$2210.11$$   | $$−32\frac{5}{9}$$ |

例如： $$32\frac{5}{9}=1×3^3+1×3^2+(−1)×3^1+(−1)×3^{−1}+(−1)×3^{−2}$$。

输入一个平衡三进制数，请将其转成对应的十进制数。

### 输入格式

在一行中输入一个平衡三进制数。

### 输出格式

在一行中输出对应的十进制数，应该是最简的带分数。

特别地，对于带分数形如 $$A\frac{B}{C}$$ 的输出的格式为 `A B C` （使用一个空格分隔）；对于带分数形如$$\frac{B}{C}$$ 的输出的格式为 `B C` （使用一个空格分隔）；对于带分数形如$$A$$ 的输出的格式为 `A` 。

**同时你需要保证 C 始终是正数。**

### 样例

{% tabs %}
{% tab title="样例1" %}
{% code title="input" %}
```
102
```
{% endcode %}

{% code title="output" %}
```
8
```
{% endcode %}
{% endtab %}

{% tab title="样例2" %}
{% code title="input" %}
```
1120.22
```
{% endcode %}

{% code title="output" %}
```
32 5 9
```
{% endcode %}
{% endtab %}

{% tab title="样例3" %}
{% code title="input" %}
```
2210.11
```
{% endcode %}

{% code title="output" %}
```
-32 5 9
```
{% endcode %}
{% endtab %}

{% tab title="样例4" %}
{% code title="input" %}
```
0.2
```
{% endcode %}

{% code title="output" %}
```
-1 3
```
{% endcode %}
{% endtab %}
{% endtabs %}

{% hint style="info" %}
**数据范围**

保证平衡三进制数长度 $$≤30$$。
{% endhint %}

### 题解

```c
#include <stdio.h>

int main(void)
{
	char s[40];
	scanf("%s", s);
	long long int integer = 0, numerator = 0, denominator = 1;
	char *p = s;
	while(*p != '.' && *p){
		integer = integer * 3;
		if(*p == '1'){
			integer += 1;
		}else if(*p == '2'){
			integer -= 1;
		}
		p ++;	
	}
	if(*p){
		p ++;
	}
	while(*p){
		denominator *= 3;
		numerator *= 3;
		if(*p == '1'){
			numerator += 1;
		}else if(*p == '2'){
			numerator -= 1;
		}
		p ++;
	}
	if(numerator == 0){
		printf("%lld", integer);
	}else if(numerator > 0){
		if(integer < 0){
			integer ++;
			numerator = denominator - numerator;
		}
		if(integer == 0){
			printf("%lld %lld", numerator, denominator);
		}else{
			printf("%lld %lld %lld", integer, numerator, denominator);
		}
	}else if(numerator < 0){
		if(integer > 0){
			integer --;
			numerator = denominator + numerator;			
		}
		if(integer == 0){
			printf("%lld %lld", numerator, denominator);
		}else {
			printf("%lld %lld %lld", integer, numerator, denominator);
		}
	}
	return 0;
}
```
