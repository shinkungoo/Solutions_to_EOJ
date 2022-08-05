# 💚 1011. i-1 进制（Easy）

**单点时限:** 2.0 sec

**内存限制:** 512 MB

在采用进位计数的数字系统中，如果使用的数码依次为 $$0,1,2,…,R−1$$，总共 $$R$$ 个数码，则称其为基 $$R$$ 数制或 $$R$$ 进制。$$R$$ 称为该数制的“基数”(Radix)，例如，十进制的基数是 $$10$$（数码为$$0,1,2,…,9$$），二进制的基数是 $$2$$（数码为$$0,1$$），八进制的基数是 $$8$$（数码为$$0,1,2,…,7$$）。

现有一种特殊的按位计数系统，该系统采用的基数为复数$$−1+i$$（其中$$i$$ 表示 $$\sqrt{−1}$$ ）。在$$−1+i$$ 进制系统中，所有“复数整数”（实部和虚部都为整数的复数，这种复数在数学上称为高斯整数）都可以表示成一个“数”，该“数”只含有$$0,1$$ 两个数码，不需要正负符号或其他常规手段，而且每个“复数整数”只有唯一一种表示方法。例如：整数 $$2$$ 用 $$−1+i$$ 进制表示出来是`1100`。

任意一个“复数整数” $$a+bi$$ （$$a,b$$ 均为整数） 可采用如下算法转换为$$−1+i$$ 进制表示：

1、$$a+bi$$ 除以 $$−1+i$$ 得到商 $$q$$ 和余数 $$r$$ ，商$$q$$也为“复数整数”，余数$$r$$ 为 $$0$$ 或 $$1$$。

​ 令 $$q=q_r+q_ii$$ （$$q_r$$ 与 $$q_i$$ 均为整数，分别表示商$$q$$的实部与虚部），

​ $$q, r$$ 必满足下式：$$a+bi=(q_r+q_ii)(−1+i)+r$$

​ 如果 $$a,b$$ 均为偶数或均为奇数，则令$$r$$ 为 $$0$$。

​ 此外，如果$$a,b$$ 一奇一偶，则令 $$r$$ 为 $$1$$。

2、重复第一步用商$$q$$ 除以 $$−1+i$$ 记录下余数$$r$$，直到商为 $$0$$，运算过程结束。

3、从下往上读取余数，就可得到 “复数整数”$$a+bi$$的$$−1+i$$进制表示。

例如：整数 $$2=2+0i$$ 的运算过程：

整数 $$2$$ 的实部和虚部都是偶数，所以余数为 $$0$$ ，$$\frac{2}{−1+i}=(−1−i)$$ 余$$0$$

$$−1−i$$ 的实部和虚部均为奇数，所以余数为$$0$$，$$\frac{−1−i}{−1+i}=i$$ 余 $$0$$

$$i$$ 的实部为偶数，虚部为奇数，所以余数为 $$1$$，$$\frac{i}{−1+i}=1$$余$$1$$

$$1$$ 的实部为奇数，虚部为偶数，所以余数为$$1$$，$$\frac{1}{−1+i}=0$$ 余 $$1$$

商为 $$0$$，运算结束，从下往上读取，得到整数$$2$$ 用$$−1+i$$进制表示为`1100`。

下表列出了$$−1+i$$ 进制的位串`0000`至`1111`对应的十进制下的“复数整数”。

| $$-1+i$$ **进制** | **十进制下的复数整数** |
| :-------------: | :-----------: |
|        0        |     $$0$$​    |
|        1        |       1       |
|        10       |    $$−1+i$$   |
|        11       |     $$i$$     |
|       100       |    $$−2i$$    |
|       101       |    $$1−2i$$   |
|       110       |    $$−1−i$$   |
|       111       |     $$−i$$    |
|       1000      |    $$2+2i$$   |
|       1001      |    $$3+2i$$   |
|       1010      |    $$1+3i$$   |
|       1011      |    $$2+3i$$   |
|       1100      |     $$2$$     |
|       1101      |     $$3$$     |
|       1110      |    $$1+i$$    |
|       1111      |    $$2+i$$    |

输入一个$$-1+i$$ 进制下的数（为了简化，我们将0,1位串用一个十六进制整数表示），输出其对应的十进制下的“复数整数” $$a+bi$$。

### 输入格式

在一行中输入一个$$−1+i$$ 进制下的数（十六进制表示，使用 0\~9 以及大写 A\~F）。

* 35% 的数据保证答案的 $$|a|,|b|≤100$$；
* 58% 的数据保证答案的 $$|a|,|b|≤2 147 483 647$$；
* 100% 的数据保证答案的 $$|a|,|b|≤9 223 372 036 854 775 807$$.

### 输出格式

在一行中输出对应的十进制下的“复数整数” $$a+bi$$（a,b均为整数），省略的情况与正常书写复数时一致，请参考上面的表格以及样例。

### 样例

{% tabs %}
{% tab title="样例1" %}
{% code title="input" %}
```
0x21
```
{% endcode %}

{% code title="output" %}
```
5-4i
```
{% endcode %}
{% endtab %}

{% tab title="样例2" %}
{% code title="input" %}
```
0x2
```
{% endcode %}

{% code title="output" %}
```
-1+i
```
{% endcode %}
{% endtab %}

{% tab title="样例3" %}
{% code title="input" %}
```
0x1C
```
{% endcode %}

{% code title="output" %}
```
-2
```
{% endcode %}
{% endtab %}

{% tab title="样例4" %}
{% code title="input" %}
```
0x9
```
{% endcode %}

{% code title="output" %}
```
3+2i
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
#include <ctype.h>

typedef long long int lli;

lli llabs(lli x)
{
    return x > 0? x : -x;
}

void PrintComplex(lli rel, lli img)
{
    if(rel != 0){
        if(rel < 0){
            printf("-");
        }
        printf("%lld", llabs(rel));
    }else if(img == 0){
        printf("0");
    }
    if(img != 0){
        if(img < 0){
            printf("-");
        }else if(img > 0 && rel != 0){
            printf("+");
        }
        if(img != 1 && img != -1){
            printf("%lld", llabs(img));
        }
        printf("i\n");
    }
}

void HexToBin(char s[], char b[])
{
    int len = strlen(s);
    int digit = 0, index = 3, count = 0;
    for(int i = 0 ; i < len ; i ++){
        if(isdigit(s[i])){
            digit = s[i] - '0';
        }else{
            digit = s[i] - 'A' + 10;
        }
        if(digit == 0){
            for(int j = 0 ; j < 4; j ++){
                b[index--] = '0';
                count ++;
            }
        }else{
            while(digit > 0 || count % 4 != 0){
                if(digit > 0){
                    b[index --] = digit % 2 + '0';
                    digit /= 2;
                }else{
                    b[index--] = '0';
                }
                count ++;
            }			
        }
        index += 8;
    }
    b[index - 3] = '\0';
}

int main(void)
{
    char s[40], bi[160];
    scanf("%s", s);
    int len = strlen(s);
    memmove(s, s + 2, len - 1);
    HexToBin(s, bi);
    lli qr = 0, qi = 0, a, b;
    for(int i = 0; bi[i]; i ++){
        lli remainder = bi[i] - '0';
        a = -qr - qi + remainder;
        b = qr - qi;
        qr = a;
        qi = b;
    }
    PrintComplex(a, b);
	
}
```
{% endcode %}
