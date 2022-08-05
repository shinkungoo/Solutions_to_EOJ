# 💛 1025. 字串非重复字符数排序

**单点时限:** 2.0 sec

**内存限制:** 256 MB

对 $$n(1≤n≤100)$$ 个由大写字母组成的长度为 $$1∽20$$ 的字符串，按字符串中不同字符个数从多到少的顺序进行排序。不同字符个数相同的字符串按字符串的字典序排序。

例如：`ABCCBA` 和 `CDFE` 按照 `CDFE` ，`ABCCBA` 顺序排序，因为 `ABCCBA` 的不同字符个数是$$3$$ 个，CDFE是$$4$$ 个。`ABCCBAX` 和 `CDFE` 按照 `ABCCBAX`, `CDFE` 顺序排序，因为这两个字符串的不同字符个数都是 $$4$$ 个，而 `ABCCBAX` 的字典序小于 `CDFE`。

### 输入格式

第 $$1$$ 行：整数$$T (1≤T≤10)$$为问题数。

第 $$2∽n+2$$ 行：第一个问题的数据。一行整数，表示$$n$$。后面$$n$$ 行中的每一行为一个字符串。

第 $$n+3∽T∗(n+1)+1$$ 行：后面问题的数据，格式与第一个问题相同。

### 输出格式

对于每个问题，输出一行问题的编号（`0` 开始编号，格式：`case #0:` 等），然后是 $$n$$ 行,在每一行中输出按要求排序后的字符串。

### 样例

{% tabs %}
{% tab title="样例1" %}
{% code title="input" %}
```
3
2
ABCCBA
CDFE
2
ABCCBAX
CDFE
3
OARWWOWVOTHDV
PRZOIMYUVVENMEFTGND
TRTDGSTGO
```
{% endcode %}

{% code title="output" %}
```
case #0:
CDFE
ABCCBA
case #1:
ABCCBAX
CDFE
case #2:
PRZOIMYUVVENMEFTGND
OARWWOWVOTHDV
TRTDGSTGO
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
	char s[21];
	int num[26];
	int kind;
}Word;

int Count(Word w)
{
	char *p = w.s;
	int ret = 0;
	while(*p){
		if(w.num[*p - 'A'] == 0){
			w.num[*p - 'A'] = 1;
			ret += 1;
		}
		p ++;
	}
	
	return ret;
}

int cmp(const void *a, const void *b)
{
	Word *pa = (Word *)a;
	Word *pb = (Word *)b;
	if(pa->kind != pb->kind){
		return pb->kind - pa->kind;
	}else{
		return strcmp(pa->s, pb->s);
	}
}

int main(void)
{
	int t;
	scanf("%d", &t);
	for(int i = 0 ; i < t; i ++){
		int n;
		scanf("%d", &n);
		Word input[n];
		for(int j = 0 ; j < n ; j ++){
			scanf("%s", input[j].s);
			memset(input[j].num, 0, sizeof(int) * 26);
			input[j].kind = Count(input[j]);
		}
		qsort(input, n, sizeof(Word), cmp);
		
		printf("case #%d:\n", i);
		for(int j = 0 ; j < n ; j ++){
			printf("%s\n", input[j].s);
		}
	}
	
	return 0;
}
```
{% endcode %}

