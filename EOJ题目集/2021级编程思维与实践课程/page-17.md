# 🧡 1022. 邮件地址排序

**单点时限:** 2.0 sec

**内存限制:** 256 MB

现接收到一大批电子邮件，邮件地址格式为：`用户名@主机域名`，要求把这些电子邮件地址做主机域名的字典序升序排序，如果主机域名相同，则做用户名的字典序降序排序。

### 输入格式

第一行输入一个正整数 n，表示共有 n 个电子邮件地址需要排序。\
接下来 n 行，每行输入一个电子邮件地址（保证所有电子邮件地址的长度总和不超过 106）。

* 对于50%的数据，保证 $$n⩽100,|s_i|⩽100$$。

用户名只包含字母数字和下划线，主机域名只包含字母数字和点。

### 输出格式

按排序后的结果输出$$n$$ 行，每行一个电子邮件地址。

### 样例

{% tabs %}
{% tab title="样例1" %}
{% code title="input" %}
```
8
23485@qq.com
rieruer@163.com
39489384@qq.com
eruie@ecnu.edu.cn
rtff@163.com
84934804@qq.com
fdll@ecnu.edu.cn
598695@qq.com
```
{% endcode %}

{% code title="output" %}
```
rtff@163.com
rieruer@163.com
fdll@ecnu.edu.cn
eruie@ecnu.edu.cn
84934804@qq.com
598695@qq.com
39489384@qq.com
23485@qq.com
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

typedef struct TNode* Tree;
typedef struct ANode* Address;
struct ANode{
	char *username;
	char *host;
};
struct TNode{
	Address item;
	Tree left;
	Tree right;
};

void split(char s1[], char s2[], char s[])
{
	char *po = s, *p = s;
	for(; *p != '@'; p++);
	memmove(s1, po, p - po);
	s1[p - po] = '\0';
	po = p;
	for(; *p; p ++);
	memmove(s2, po, p - po);
	s2[p - po] = '\0';
}

int cmp(Address a, Address b)
{
	if(strcmp(a->host, b->host) != 0){
		return strcmp(a->host, b->host);
	}else{
		return strcmp(b->username, a->username);
	}
}

Address CreateAddress(char s[])
{
	Address temp = (Address)malloc(sizeof(struct ANode));
	char s1[1000000], s2[1000000];
	split(s1, s2, s);
	temp->username = (char *)malloc(strlen(s1) + 1);
	temp->host = (char *)malloc(strlen(s2) + 1);
	strcpy(temp->username, s1);
	strcpy(temp->host, s2);
	return temp;
}

Tree CreateTree(char s[])
{
	Tree temp;
	temp = (Tree)malloc(sizeof(struct TNode));
	temp->item = CreateAddress(s);
	temp->left = NULL, temp->right = NULL;
	return temp;
}

Tree Insert(Tree t, Address a)
{
	int c;
	if(!t){
		t = (Tree)malloc(sizeof(struct TNode));
		t->item = a;
		t->left = NULL, t->right = NULL;
	}else{
		if((c = cmp(a, t->item)) < 0){
			t->left = Insert(t->left, a);
		}else if(c > 0){
			t->right = Insert(t->right, a);
		}else{
			t->left = Insert(t->left, a);
		} 
	}
	
	return t;
}

void PrintTree(Tree t)
{
	if(t){
		PrintTree(t->left);
		printf("%s%s\n", t->item->username, t->item->host);
		PrintTree(t->right);
	}
}

int main(void)
{
	int n;
	scanf("%d", &n);
	char s[1000001];
	scanf("%s", s);
	Tree words = CreateTree(s);
	for(int i = 1; i < n ; i ++){
		scanf("%s", s);
		Address temp = CreateAddress(s);
		words = Insert(words, temp);
	}
	PrintTree(words);	

	return 0;
}
```
{% endcode %}
