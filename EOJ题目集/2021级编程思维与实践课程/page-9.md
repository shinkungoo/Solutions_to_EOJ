# 💙 1023. 字串排序

**单点时限:** 2.0 sec

**内存限制:** 256 MB

在 2010 年百度公司的一次校园招聘笔试中，要求应聘者设计一个 strnumcmp 函数。对比普通的 strcmp 函数，差别在于，当字符串中包含数字时，比较数字大小。数字大小相同或不含数字时，仍然沿用原来的 strcmp 方式。所有不含数字的字符串均小于含数字的字符串。每个字符串的长度范围为 1 \~ 30，而其中包含的数字个数范围为 0 \~ 8，且数字在一个字符串中是连续的。

例如：strnumcmp 的判定结果：

`"abc"<"abc#"<"abcd"<"abc1"<"abc2"<"abc10"`

而一般的 strcmp 的判定结果：

`"abc"<"abc#"<"abc1"<"abc10"<"abc2"<"abcd"`

写一个程序，用 strnumcmp 函数对一组字符串按升序排序。

### 输入格式

$$n$$ 个由一个空格分隔的字符串 $$(1⩽n⩽100)$$

### 输出格式

排序后的 $$n$$ 个字符串，两个字符串之间用一个空格分开。

### 样例

{% tabs %}
{% tab title="样例1" %}
{% code title="input" %}
```
abc# abc1 abc10 abcd abc2 abc
```
{% endcode %}

{% code title="output" %}
```
abc abc# abcd abc1 abc2 abc10
```
{% endcode %}


{% endtab %}
{% endtabs %}

{% hint style="info" %}
#### 提示

1. 字符串中可能包含0，如”a0”。并且”a0”>”zzzz”。
2. 调用scanf函数时，正确读取数据时返回读入数据项的个数，小于1时通常表示数据读完或读取出错。
3. 在Windows环境中运行程序时，输入Control+Z后输入回车表示输入流的结束。
{% endhint %}

### 题解

{% code lineNumbers="true" %}
```c
#include <stdio.h>
#include <ctype.h>
#include <stdlib.h>
#include <string.h>

typedef struct TNode* Tree;
typedef long long int lli;
struct TNode{
	char s[40];
	Tree left;
	Tree right;
};

int NoDigit(char s[])
{
	char *p = s;
	int ret = 1;
	while(*p){
		if(isdigit(*p)){
			ret = 0;
			break;
		}
		p ++;
	}
	return ret;
}

lli GetNumber(char s[])
{
	char *p = s;
	lli ret = 0;
	for(; !isdigit(*p) && *p; p ++);
	while(*p && isdigit(*p)){
		ret = ret * 10 + (*p - '0');
		p ++;
	}
	
	return ret;
}

int strnumcmp(char s1[], char s2[])
{
	if(NoDigit(s1) && NoDigit(s2)){
		return strcmp(s1, s2);
	}else if(NoDigit(s1)){
		return -1;
	}else if(NoDigit(s2)){
		return 1;
	}else if(GetNumber(s1) != GetNumber(s2)){
		return GetNumber(s1) > GetNumber(s2) ? 1 : -1; 
	}else{
		return strcmp(s1, s2);
	}
}

Tree CreateTree(char s[])
{
	Tree temp;
	temp = (Tree)malloc(sizeof(struct TNode));
	strcpy(temp->s, s);
	temp->left = NULL, temp->right = NULL;
	return temp;
}

Tree Insert(Tree t, char s[])
{
	int cmp;
	if(!t){
		t = (Tree)malloc(sizeof(struct TNode));
		strcpy(t->s, s);
		t->left = NULL, t->right = NULL;
	}else{
		if((cmp = strnumcmp(s, t->s)) < 0){
			t->left = Insert(t->left, s);
		}else if(cmp > 0){
			t->right = Insert(t->right, s);
		}else{
			t->left = Insert(t->left, s);
		} 
	}
	
	return t;
}

void PrintTree(Tree t)
{
	if(t){
		PrintTree(t->left);
		printf("%s ", t->s);
		PrintTree(t->right);
	}
}

int main(void)
{
	char temp[40];
	scanf("%s", temp);
	Tree words = CreateTree(temp);
	while(scanf("%s", temp) != EOF){
		words = Insert(words, temp);
	}
	PrintTree(words);
	
	return 0;
}

```
{% endcode %}
