# 💙 1021. 随机排序

**单点时限:** 2.0 sec

**内存限制:** 256 MB

给定一组以一个空格分隔的只含大小写字母的字符串。与普通字典序不同，按照给定的字母顺序对这组字符串排序。

设两个字符串的字母不会完全相同。如：Hat、hat、HAt 等不会同时出现。

例如：\
字母顺序为 QWERTYUIOPASDFGHJKLZXCVBNM 时:

一组字符串 hat cat bat book bookworm Dallas Austin Houston fire firefox fumble

排序结果为：Austin Dallas fumble fire firefox Houston hat cat book bookworm bat

### 输入格式

每组数据由 2 行组成:

第 1 行为字母顺序（26 个大写字母），第 2 行是需要排序的一组字符串（只含大小写字母，长度不大于 20）。

数据不多于 100 组。需要排序的一组字符串中包含的字符串个数至少 1 个，至多 100 个。

### 输出格式

对于每一组数据，输出排序后的字符串。字符串之间输出一个空格，最后一个字符串后面没有空格，而是输出一个换行符。

### 样例

{% tabs %}
{% tab title="样例1" %}
{% code title="input" %}
```
QWERTYUIOPASDFGHJKLZXCVBNM
hat cat bat book bookworm Dallas Austin Houston fire firefox fumble
QWERTYUIOPASDFGHJKLZXCVBNM
How are you
QAZWSXEDCRFVTGBYHNUJMIKOLP
How are you
ABCDEFGHIJKLMNOPQRSTUVWXYZ
How are you
```
{% endcode %}

{% code title="output" %}
```
Austin Dallas fumble fire firefox Houston hat cat book bookworm bat
you are How
are you How
are How you
```
{% endcode %}


{% endtab %}
{% endtabs %}

### 题解

{% code lineNumbers="true" %}
```c
#include <stdio.h>
#include <stdlib.h>
#include <ctype.h>

int map[26];
typedef struct{
	char s[30];
}Word;
void CreateMap(char s[])
{
	char *p = s, index = 0;
	while(*p){
		map[*p - 'A'] = index;
		p ++, index ++;
	}
}

int cmp(const void *a, const void *b)
{
	Word *pa = (Word *)a;
	Word *pb = (Word *)b;
	char *ppa = pa->s, *ppb = pb->s; 
	for(; toupper(*ppa) == toupper(*ppb); ppa ++, ppb ++);
	if(!*ppa){
		return -1;
	}else if(!*ppb){
		return 1;
	}else{
		return map[toupper(*ppa) - 'A'] - map[toupper(*ppb) - 'A'];
	}
}

int main(void)
{
	char maps[27];
	scanf("%s", maps);
	CreateMap(maps);
	char c = getchar();
	int index = 0, line = 0;
	Word words[100];
	while((c = getchar()) != EOF){
		if(c == ' '){
			words[line].s[index] = '\0';
			line ++;
			index = 0;
		}else if(c == '\n'){
			words[line].s[index] = '\0';
			qsort(words, line + 1 , sizeof(Word), cmp);
			for(int i = 0 ; i < line + 1; i ++){
				printf("%s%c", words[i].s, i == line ? '\n' : ' ');
			}
			scanf("%s", maps);
			CreateMap(maps);
			c = getchar();
			line = 0, index = 0;
		}else{
			words[line].s[index] = c;
			index ++;			
		}
	}
	
	return 0;
}
```
{% endcode %}
