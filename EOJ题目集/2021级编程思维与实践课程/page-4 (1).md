# 💚 1028. 排序去重

**单点时限:** 2.0 sec

**内存限制:** 256 MB

有 $$n$$ 个 $$1$$ 到$$1000$$ 之间的整数 $$(1≤n≤100)$$，对于其中重复的数字，只保留一个，把其余相同的数去掉。然后再按照指定的排序方式把这些数排序。

### 输入格式

第 1 行为字母 `A` 或 `D`，`A` 表示按照升序排序，`D` 表示按照降序排序。

第 2 行开始有若干个用一个空格或换行符分隔的正整数。

### 输出格式

相互之间用一个空格分隔的经去重和排序后的正整数。最后一个数后没有空格。

### 样例

{% tabs %}
{% tab title="样例1" %}
{% code title="input" %}
```
A
20 40 32 67 40 20 89 300 400 15
```
{% endcode %}

{% code title="output" %}
```
15 20 32 40 67 89 300 400
```
{% endcode %}
{% endtab %}
{% endtabs %}

### 题解

{% code lineNumbers="true" %}
```c
#include <stdio.h>

void swap(int a[], int i, int j)
{
	int temp;
	temp = a[i];
	a[i] = a[j];
	a[j] = temp;
}

void qsort_up(int a[], int left, int right)
{
	int i, last;
	if(left >= right){
		return ;
	}
	swap(a, left, (left + right) / 2);
	last = left;
	for(i = left + 1; i <= right; i++){
		if(a[i] < a[left]){
			swap(a, i, ++last);
		}
	}
	swap(a, left, last);
	qsort_up(a, left, last - 1);
	qsort_up(a, last + 1, right);
}

void qsort_down(int a[], int left, int right)
{
	int i, last;
	if(left >= right){
		return ;
	}
	swap(a, left, (left + right) / 2);
	last = left;
	for(i = left + 1; i <= right; i++){
		if(a[i] > a[left]){
			swap(a, i, ++last);
		}
	}
	swap(a, left, last);
	qsort_down(a, left, last - 1);
	qsort_down(a, last + 1, right);
}

int rmvdup(int a[], int n)
{
	int ret;
	if(n == 0){
		ret = 0;
	}else{
		int *a1, *a2;
		a1 = a;
		a2 = a1 + 1;
		ret = 1;
		for(int i = 1; i < n; i ++){
			if(*a1 != *a2){
				a1 += 1;
				*a1 = *a2;
				ret ++;
			}
			a2 += 1;
		}
	}
	
	return ret;
}

int main(void)
{
	char c;
	int input[1001], num = 0, num_del;
	scanf("%c", &c);
	while(scanf("%d", &input[num++]) != EOF);
	if(c == 'A'){
		qsort_up(input, 0, num - 2);
	}else if(c == 'D'){
		qsort_down(input, 0, num - 2);
	}
	num_del = rmvdup(input, num - 1);
	for(int i = 0 ; i < num_del; i ++){
		printf("%d%c", input[i], (i == num_del - 1) ? '\n':' ');
	}
	
	
	return 0;
}

```
{% endcode %}
