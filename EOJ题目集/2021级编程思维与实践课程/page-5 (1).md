# 💛 1027. 极坐标排序

**单点时限:** 2.0 sec

**内存限制:** 256 MB

在平面上，确定一个点的位置通常有下面两种表示方法：

![](https://acm.ecnu.edu.cn/upload/3059/3059\_1.png)

当极坐标系中的极点 $$O$$ 与直角坐标系中的原点 $$O$$ 重合，极轴 $$OX$$ 与直角坐标系中的$$X$$ 轴的正半轴重合，并且两种坐标系的单位长度相同，那么平面内任意一点 $$P$$ 的直角坐标与极坐标可以互相转换。

![](https://acm.ecnu.edu.cn/upload/3059/3059\_2.png)

例如：

点 p 直角坐标为：$$(1，1)$$，则对应的极坐标为：$$(1.4142,\frac{π}{4})$$。

点 p 直角坐标为：$$(−1，1)$$，则对应的极坐标为：$$(1.4142,\frac{3π}{4})$$。

点 p 直角坐标为：$$(−1，−1)$$，则对应的极坐标为：$$(1.4142,\frac{5π}{4})$$。

点 p 直角坐标为：$$(1，−1)$$，则对应的极坐标为：$$(1.4142,\frac{7π}{4})$$。

点 p 直角坐标为：$$(0，1)$$，则对应的极坐标为：$$(1,\frac{π}{2})$$。

点 p 直角坐标为：$$(1，0)$$，则对应的极坐标为：$$(1,0)$$。

给出$$N$$ 个点的直角坐标 $$(x,y)$$，请计算出这些点对应的极坐标，将这$$N$$ 个点按照极角$$θ$$ 从小到大排序，如果两个点的极角相同，则将它们按照极径$$ρ$$ 由大到小排序。

注意：$$ρ≥0$$，极角 $$0≤θ<2π$$

### 输入格式

第 1 行：整数 $$T (1≤T≤10)$$ 为问题数。

对于每个问题，按如下格式输入：

第 1 行：输入一个正整数$$N(1≤N≤1000)$$，表示点的个数；

接下来 $$N$$ 行，每行输入两个浮点数$$x,y$$，表示点的直角坐标，两个数之间由一个空格分隔。

### 输出格式

对于每个问题，输出一行问题的编号（`0` 开始编号，格式：`case #0:` 等）。

接下来 N行输出排序后的点的极坐标$$(ρ,θ)$$，每行输出一个点的极坐标，小数点后保留$$4$$ 位，极角采用弧度表示。

### 样例

{% tabs %}
{% tab title="样例1" %}
{% code title="input" %}
```
3
5
1.0 1.0
2.0 2.0
-1.0 1.0
0 1.0
1.0 0
1
0 -1.0
6
1.0 1.0
0 1.0
1.0 0
-1.0 1.0
-1.0 -1.0
1.0 -1.0
```
{% endcode %}

{% code title="output" %}
```
case #0:
(1.0000,0.0000)
(2.8284,0.7854)
(1.4142,0.7854)
(1.0000,1.5708)
(1.4142,2.3562)
case #1:
(1.0000,4.7124)
case #2:
(1.0000,0.0000)
(1.4142,0.7854)
(1.0000,1.5708)
(1.4142,2.3562)
(1.4142,3.9270)
(1.4142,5.4978)
```
{% endcode %}
{% endtab %}
{% endtabs %}

### 题解

{% code lineNumbers="true" %}
```c
#include <stdio.h>
#include <math.h>
#include <stdlib.h>

const double ep = 1e-8;
const double pi = 3.1415926;

typedef struct{
	double x;
	double y;
	double r;
	double th;
}Point;

int cmp(const void *a, const void *b)
{
	Point *pa = (Point *)a;
	Point *pb = (Point *)b;
	int temp;
	if(fabs(pa->th - pb->th) > ep){
		return pa->th - pb->th > ep  ? 1 : -1;
	}else{
		return pa->r - pb->r < ep  ? 1 : -1;
	}
}

int main(void)
{
	int t;
	scanf("%d", &t);
	for(int i = 0 ; i < t; i ++){
		int n;
		scanf("%d", &n);
		Point in[n];
		for(int j = 0 ; j < n ; j ++){
			scanf("%lf %lf", &in[j].x, &in[j].y);
			in[j].r = sqrt(in[j].x * in[j].x + in[j].y * in[j].y);
			in[j].th = atan2(in[j].y, in[j].x);
			if(in[j].th < 0){
				in[j].th += 2 * pi;
			}
		}
//		for(int j = 0 ; j < n ; j ++){
//			scanf("%lf %lf", &in[j].r, &in[j].th);
//		}
		qsort(in, n, sizeof(Point),cmp);
		printf("case #%d:\n", i);
		for(int j = 0 ; j < n; j ++){
			printf("(%.4f,%.4f)\n", in[j].r, in[j].th);
		}
	}
	return 0;
}
```
{% endcode %}
