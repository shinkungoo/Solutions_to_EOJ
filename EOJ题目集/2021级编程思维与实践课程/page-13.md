# 🧡 1018. 素数进制

**单点时限:** 2.0 sec

**内存限制:** 256 MB

进制也就是进位制，是人们规定的一种进位方法。对于任何一种进制：`X`进制表示某一位置上的数运算时是逢`X`进一位。

`十`进制是逢`十`进一，`十六`进制是逢`十六`进一，`二`进制就是逢`二`进一，以此类推，`X`进制就是逢`X`进一。

现定义一种新的进制表示方法`素数`进制表示法。

`素数`进制不是单一进制，在`素数`进制中，每一位的进制都各不相同，具体定义如下：

1、将素数从小到大排列成一个素数列表：$$2,3,5,7,11,13,17,19,23,29,31,⋯$$

2、在`素数`进制表示法中，从右边最低位开始，第$$n$$位的进制定义为素数列表中第$$n$$个素数

3、第$$n$$ 位可用的数码为$$0$$ \~ 第个素数(第$$n$$个素数$$−1$$)

4、第$$n$$ 位对应的位权为素数列表中前$$n−1$$ 个素数的乘积

5、 第$$n$$ 位上的数在运算时逢该素数进一位。

即：右边最低位个位（第$$1$$位）的进制为`2`进制 (个位上可用的数码为0,1)，对应的位权为$$1$$；十位（第 $$2$$位）的进制为`3`进制（十位上可用的数码为$$0,1,2$$ ），对应的位权为 $$2$$ ；百位的进制为`5`进制（百位上可用的数码为 $$0,1,2,3,4$$ ），对应的位权为$$4$$ ……

例如：

十进制整数$$2$$ 的`素数`进制表示为 $$10$$ ，记为：$$1,0$$

十进制整数 $$38$$ 的`素数`进制表示为 $$1110$$ ，记为：$$1,1,1,0$$

给定一个素数进制表示的非负整数，输出其对应的十进制表示。

### 输入格式

在一行中输入一个不超过$$25$$ 位的素数进制表示的非负整数$$A$$ ，相邻两位数码用逗号分隔。

### 输出格式

在一行中输出 $$A$$ 的十进制值，不包含前导$$0$$ 。

{% tabs %}
{% tab title="样例1" %}
{% code title="input" %}
```
1,1,1,0
```
{% endcode %}

{% code title="output" %}
```
38
```
{% endcode %}
{% endtab %}

{% tab title="样例2" %}
{% code title="input" %}
```
46,42,40,36,30,28,22,18,16,12,10,6,4,2,1
```
{% endcode %}

{% code title="output" %}
```
614889782588491409
```
{% endcode %}
{% endtab %}

{% tab title="Untitled" %}
{% code title="input" %}
```
96,88,82,78,72,70,66,60,58,52,46,42,40,36,30,28,22,18,16,12,10,6,4,2,1
```
{% endcode %}

{% code title="output" %}
```
2305567963945518424753102147331756069
```
{% endcode %}


{% endtab %}
{% endtabs %}

{% hint style="info" %}
**数据分布：**

20分数，入不超过15位素数进制数字，十进制结果不超过 $$10^{18}$$

80分数，输入不超过25位素数进制数字
{% endhint %}

### 题解

{% code lineNumbers="true" %}
```c
#include <stdio.h>
#include <string.h>

const int primes[27] = {97,91,89,83,79,73,71,67,61,59,53,47,43,41,37,31,29,23,19,17,13,11,7,5,3,2,1};
const int LEN = 100;

void ReadNumber(char s[], int num[])
{
	char *p = s;
	int temp = 0, index = 0;
	while(*p){
		if(*p == ','){
			num[index++] = temp;
			temp = 0;
		}else{
			temp = temp * 10 + (*p - '0');	
		}
		p ++;
	}
	num[index++] = temp;
	memmove(num + (26 - index), num, sizeof(int) * index);
	memset(num, -1, sizeof(int) * (26 - index));
}

void Addition(char s1[], char s2[], char res[])
{
	for(int i = LEN - 1; i >= 0 ; i --){
		res[i] = res[i] + s1[i] + s2[i] - 3 * '0';
		if(res[i] >= 10){
			res[i] -= 10;
			res[i - 1] += 1;
		}
		res[i] += '0';
	}
}

void Multiply(char s1[], char s2[], char res[])
{
	int len = 0;
	while(s2[len] == '0'){
		len ++;	
	}
	int temp;
	if(LEN - len == 2){
		char s21[LEN], s22[LEN];
		char res1[LEN], res2[LEN], s10[LEN];
		memset(s21, '0', sizeof(s21));
		memset(s22, '0', sizeof(s22));
		s21[LEN - 1] = s2[LEN - 1];
		s22[LEN - 1] = s2[LEN - 2];
		memset(res1, '0', sizeof(res1));
		memset(res2, '0', sizeof(res2));
		memmove(s10, s1 + 1, LEN - 1);
		s10[LEN - 1] = '0';
		Multiply(s1, s21, res1);
		Multiply(s10, s22, res2);
		Addition(res1, res2, res);
	}else if(LEN - len == 1){
		for(int i = LEN - 1; i >= 0; i --){
			temp = (s1[i] - '0') * (s2[LEN - 1] - '0');
			res[i] += temp % 10;
			if(temp >= 10){
				res[i - 1] += temp / 10;
			}
		}
	}
}

void Parallel(int n, char res[])
{
	for(int i = LEN - 1; i >= 0; i --){
		res[i] = n % 10 + '0';
		n /= 10;
	}
}

void PrimeToDec(int num[], char res[])
{
	char temp1[LEN], temp2[LEN], temp3[LEN], temp4[LEN];
	memset(temp2, '0', sizeof(temp2));
	memset(temp3, '0', sizeof(temp3));
	memset(temp4, '0', sizeof(temp4));
	Parallel(0, temp1);
	for(int i = 0 ; i < 26; i ++){
		if(num[i] != -1){
			Parallel(num[i], temp2);
			Addition(temp1, temp2, temp3);
			Parallel(primes[i + 1], temp4);
			memset(temp1, '0', sizeof(temp1));
			Multiply(temp3, temp4, temp1);
			memset(temp3, '0', sizeof(temp3));	
		}
	}
	int len = 0;
	while(temp1[len] == '0'){
		len ++;
	}
	memmove(res, temp1 + len, LEN - len);
	res[LEN - len] = '\0';
}

int main(void)
{
	char s[LEN], res[LEN];
	int num[26];
	memset(num, -1, sizeof(num));
	memset(res, '0', sizeof(res));
	scanf("%s", s);
	if(strcmp(s, "0") == 0){
		printf("0");
	}else{
		ReadNumber(s, num);
		PrimeToDec(num, res);
		printf("%s", res);		
	}
	return 0;
}
```
{% endcode %}
