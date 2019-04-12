[Monkey and Banana](https://vjudge.net/contest/293713#problem/C)

##### 题目意思:给定n个长方体,通过不同摆放叠加上去,求最高高度,而且叠上去的那个长方体的长和宽必须大于当前长方体的长和宽.

##### 做法就是 输入的x,y,z先排序,然后arr数组中的x必须大过y,因为x是长,y是宽,然后总共有6种长方体,但因为x是长,y是宽,所以适合的只有3种,dp[0]就是arr[0]的高度z,然后开始dp,类型就像求最长递增子序列一样,定义一个maxH,选择的条件:i的长方体的长和宽必须比j的长和宽大.

    #include <stdio.h>
    #include <algorithm>

    using namespace std;

    struct node {
        int x, y, z;
    } arr[95];

    int cmp(node a, node b){
        if (a.y == b.y) return a.x < b.x;
        return a.y < b.y;
    }

    int n, idx = 1;
    int len[3], dp[95];

    int main() {
        while (scanf("%d", &n) && n != 0) {
            for (int i = 0; i < n; ++i) {
                scanf("%d %d %d", &len[0], &len[1], &len[2]);
                sort(len, len + 3);
                arr[i * 3].x = len[0], arr[i * 3].y = len[1], arr[i * 3].z = len[2];
                arr[i * 3 + 1].x = len[1], arr[i * 3 + 1].y = len[2], arr[i * 3 + 1].z = len[0];
                arr[i * 3 + 2].x = len[0], arr[i * 3 + 2].y = len[2], arr[i * 3 + 2].z = len[1];
            }
            sort(arr, arr + n * 3, cmp);
            fill(dp, dp + n * 3, 0);
            dp[0] = arr[0].z;
            int maxLen = 0;
            for (int i = 1; i < n * 3; ++i) {
                int maxH = 0;
                for (int j = 0; j < i; ++j) {
                    if (arr[j].x < arr[i].x && arr[j].y < arr[i].y){
                        maxH = max(maxH, dp[j]);
                    }
                }
                dp[i] = maxH + arr[i].z;
                maxLen = max(maxLen, dp[i]);
            }
            printf("Case %d: maximum height = %d\n", idx++, maxLen);
        }
        return 0;
    }
