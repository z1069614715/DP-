[Phalanx](https://vjudge.net/contest/293713#problem/Q)

##### 最大对称矩阵,需要注意的是对角线是从左下到右上的.i=0和j=n-1时可以跳过,因为是边界,检测当前这个坐标的上和右相同的元素个素,如果比右上方的大,则当前坐标的最大对称矩阵等于右上方+1,否则就是当前相同元素个数,还有一个点就是需要定义一个maxL值去记录最大的对称矩阵.

    #include <stdio.h>
    #include <algorithm>

    using namespace std;

    int n;
    char arr[1005][1005];
    int dp[1005][1005];

    int main() {
        while (scanf("%d", &n) && n != 0) {
            int maxV = 1;
            for (int i = 0; i < n; ++i) {
                scanf("%s", &arr[i]);
            }
            fill(dp[0], dp[0] + 1005 * 1005, 1);
            for (int i = 1; i < n; ++i) {
                for (int j = 0; j < n - 1; ++j) {
                    int a = 0, b = 0;
                    while (i - a >= 0 && j + b <= n - 1 && arr[i - a][j] == arr[i][j + b]) {
                        ++a;
                        ++b;
                    }
                    if (a > dp[i - 1][j + 1]) dp[i][j] = dp[i - 1][j + 1] + 1;
                    else dp[i][j] = a;
                    maxV = max(maxV, dp[i][j]);
                }
            }
            printf("%d\n", maxV);
        }
        return 0;
    }
