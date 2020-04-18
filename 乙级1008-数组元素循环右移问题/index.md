# 【乙级】1008 数组元素循环右移问题（20）


一个数组*A*中存有*N*（>0）个整数，在不允许使用另外数组的前提下，将每个整数循环向右移*M*（≥0）个位置，即将*A*中的数据由（*A*0*A*1⋯*A**N*−1）变换为（*A**N*−*M*⋯*A**N*−1*A*0*A*1⋯*A**N*−*M*−1）（最后*M*个数循环移至最前面的*M*个位置）。如果需要考虑程序移动数据的次数尽量少，要如何设计移动的方法？

### 输入格式:

每个输入包含一个测试用例，第1行输入*N*（1 ≤ *N* ≤ 100）和*M*（≥ 0）；第2行输入*N*个整数，之间用空格分隔。

### 输出格式:

在一行中输出循环右移 *M* 位以后的整数序列，之间用空格分隔，序列结尾不能有多余空格。

### 输入样例:

```in
6 2
1 2 3 4 5 6
```

### 输出样例:

```out
5 6 1 2 3 4
```

分析： 若 M ＜ N，则移动 M 个。若 M = N，则不用移动。若 M ＞ N，则移动 M % N 个。我选择了分两次读入，第一次读入 M - N 个数，存储在 N 到 M - 1 的位置。第二次读入 N 个数，存储在 0 到 N - 1 的位置，下面附上第一次代码：

```c++
# include <iostream>
# include <vector>
using namespace std;
int main()
{
    int M, N;
    cin >> N >> M;
    vector<int> v(N);
    int flag = N == M ? 0 : N > M ? 1 : 2;	/* 0 代表 N = M, 1 代表 N > M, 2 代表 N < M */
    switch(flag){
        case 0:
            for(int i = 0; i < N; i++)	/* N = M 直接读取就好 */
                cin >> v[i];
            break;
        case 1:
            for(int i = M; i < N; i++)	/* N > M 第一次读取，从 M 开始，到 N-1 结束 */
                cin >> v[i];
            for(int i = 0; i < M; i++)	/* N > M 第二次读取，从 0 开始，到 M-1 结束 */
                cin >> v[i];
            break;
        case 2:
            M %= N;
            for(int i = M; i < N; i++)	/* N < M 第一次读取，从 M 开始，到 N-1 结束 */
                cin >> v[i];
            for(int i = 0; i < M; i++)	/* N < M 第二次读取，从 0 开始，到 M-1 结束 */
                cin >> v[i];
            break;
    }
    for(int i = 0; i < N - 1; i++)		/* 输出 */
        cout << v[i] << " ";
    cout << v[N - 1];
    return 0;
}
```

读了柳神的解法之后，我将自己的代码精简了一下：

```c++
# include <iostream>
# include <vector>
using namespace std;
int main()
{
    int m, n;
    cin >> n >> m;
    vector<int> v(n);
    m %= n;
    if( m != 0){	/* m < 0 或 m > 0 的情况 */
        for(int i = m; i < n; i++)	/* 第一次读取，从 m 开始，到 n-1 结束 */
            cin >> v[i];
        for(int i = 0; i < m; i++)	/* 第二次读取，从 0 开始，到 m-1 结束 */
            cin >> v[i];
    }else{	/* m = 0 的情况 */
		for(int i = 0; i < n; i++)	/* 顺序读取 */
            cin >> v[i];
    }
    for(int i = 0; i < n - 1; i++)
        cout << v[i] << " ";
    cout << v[n - 1];
    return 0;
}
```

附上柳神的解法 [原文地址](http://www.liuchuo.net/archives/522)：

```c++
#include <iostream>
#include <algorithm>
#include <vector>
using namespace std;
int main() {
    int n, m;
    cin >> n >> m;
    vector<int> a(n);
    for (int i = 0; i < n; i++)
        cin >> a[i];
    m %= n;
    if (m != 0) {
        reverse(begin(a), begin(a) + n);
        reverse(begin(a), begin(a) + m);
        reverse(begin(a) + m, begin(a) + n);
    }
    for (int i = 0; i < n - 1; i++)
        cout << a[i] << " ";
    cout << a[n - 1];
    return 0;
}
```

总结：

​	当代码的冗余度较高的时候，则应将重复代码写到一起，而后改进自己的算法，进一步抽象自己的解。
