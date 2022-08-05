# 💚 1017. 罗马数字

**单点时限:** 2.0 sec

**内存限制:** 512 MB

罗马数字是古罗马使用的记数系统，现今仍很常见。 罗马数字有七个基本符号：`I` 、`V`、`X`、`L`、`C`、`D`、`M`。

|   罗马数字   |  I  |  V  |  X  |  L  |  C  |  D  |   M  |
| :------: | :-: | :-: | :-: | :-: | :-: | :-: | :--: |
| 对应的阿拉伯数字 |  1  |  5  |  10 |  50 | 100 | 500 | 1000 |

需要注意的是罗马数字与十进位数字的意义不同，它没有表示零的数字，与进位制无关。 按照下述的规则可以表示任意正整数。

**“标准”形式**

**重复数次**：一个罗马数字重复几次，就表示这个数的几倍。\
**右加左减**：

* 在较大的罗马数字的右边记上较小的罗马数字，表示大数字加小数字。
* 在较大的罗马数字的左边记上较小的罗马数字，表示大数字减小数字。
* 左减的数字有限制，仅限于 `I`、`X`、`C`。例如，$$45$$ 表示为 `XLV`，而不是 `VL`。
* 左减时不可跨越一个位值。例如，$$99$$ 是 `XCIX`( $$[100−10]+[10−1]$$)，而不是 `IC`($$[100−1]$$)。等同于阿拉伯数字每位数字分别表示。
* 左减数字必须为一位，比如$$8$$ 是 `VIII` , 而不用 `IIX`。
* 罗马数字 `V`、`L`、`D` 中的任何一个放在较大的罗马数字右边只能使用一个。

**加数乘千**：在罗马数字的上方加上一条横线，表示将这个数乘以 $$1000$$，即是原数的$$1000$$ 倍。同理，如果上方有两条横线，即是原数的 $$1000000$$。

现在给出一个罗马数，输出其对应的阿拉伯数。注意，罗马数字上面的一条横线，用括号表示。如果有两条横线，则用两个括号表示。具体见输入样例。

### 输入格式

输入包含一个字符串，表示给出的罗马数。

### 输出格式

输出一个整数，表示答案。

### 样例

{% tabs %}
{% tab title="样例1" %}
{% code title="input" %}
```
III
```
{% endcode %}

{% code title="output" %}
```
3
```
{% endcode %}
{% endtab %}

{% tab title="样例2" %}
{% code title="input" %}
```
CXCIX
```
{% endcode %}

{% code title="output" %}
```
199
```
{% endcode %}
{% endtab %}

{% tab title="样例3" %}
{% code title="input" %}
```
CCCXCV
```
{% endcode %}

{% code title="output" %}
```
395
```
{% endcode %}
{% endtab %}

{% tab title="样例4" %}
{% code title="input" %}
```
MMMCCCXXXIII
```
{% endcode %}

{% code title="output" %}
```
3333
```
{% endcode %}
{% endtab %}

{% tab title="样例5" %}
{% code title="input" %}
```
M(V)
```
{% endcode %}

{% code title="output" %}
```
4000
```
{% endcode %}
{% endtab %}

{% tab title="样例6" %}
{% code title="input" %}
```
((VI))
```
{% endcode %}

{% code title="output" %}
```
6000000
```
{% endcode %}


{% endtab %}
{% endtabs %}

{% hint style="info" %}
**数据范围**

对于所有数据，保证输出的答案在 $$[1,10^{18}]$$ 范围内，且输入的字符串长度$$≤50$$。

20分数，证不存在左减的情况

40分数，保证不存在加数乘千的情况

40分数，没有特殊限制
{% endhint %}

### 题解

{% code lineNumbers="true" %}
```c
#include <stdio.h>
#include <ctype.h>
#include <string.h>

typedef long long int lli;

int ReadRoman(char c)
{
	int ret;
	switch(c){
		case 'I':	ret = 1;	break;
		case 'V':	ret = 5;	break;
		case 'X':	ret = 10;	break;
		case 'L':	ret = 50;	break;
		case 'C':	ret = 100;	break;
		case 'D':	ret = 500;	break;
		case 'M':	ret = 1000;	break;
	}
	
	return ret;
}

int isMinus(lli n)
{
	int ret = 0;
	if(n == 1 || n % 5 == 0){
		ret = 1;
	}
	
	return ret;
}

int main(void)
{
	char s[60];
	lli num[60], index = 0, multi = 1, temp = 0, res = 0;
	scanf("%s", s);
	char *p = s; 
	while(*p){
		if(isalpha(*p)){
			num[index ++] = ReadRoman(*p) * multi;
		}else if(*p == '('){
			multi *= 1000;
		}else if(*p == ')'){
			multi /= 1000;
		}
		p ++;
	}
	for(int i = 0 ; i < index; i++){
		if(i != index - 1 && isMinus(num[i]) && num[i] < num[i + 1]){
			res -= num[i];
		}else{
			res += num[i];
		}
	}
	printf("%lld", res);
	
	
	return 0;
}
```
{% endcode %}

