[Help Jimmy](http://poj.org/problem?id=1661)

##### 题目意思:场景中包括多个长度和高度各不相同的平台。地面是最低的平台，高度为零，长度无限。 Jimmy老鼠在时刻0从高于所有平台的某处开始下落，它的下落速度始终为1米/秒。当Jimmy落到某个平台上时，游戏者选择让它向左还是向右跑，它跑动的速度也是1米/秒。当Jimmy跑到平台的边缘时，开始继续下落。Jimmy每次下落的高度不能超过MAX米，不然就会摔死，游戏也会结束。

##### 其中一个注意的点就是落在平台边缘上也算是平台上,但再踏出一步就开始下落,意思就是不需要再加1,这一步可以忽略.

##### 题目做法:有左右的走法,使用记忆化搜索,高度从低到高排序,arr[0]设为开始下落的点.

##### dfs函数就是记忆化搜索的过程,如果这个值已经访问过就返回这个值,否则进去判断,设置一个minTime和可不可以跳的变量isJump,当这块板的高度与开始下落的高度相差大于maxH的话,就break,或者跳到第一块接触的板就进行左右走,获取最小值,然后就break,同时设置isJump为0,,意思就是已跳.需要注意一个就是当这块板是最后一块板的时候,isJump的值肯定一直是1,而且题目保证一定有解,所以判断isJump && 跳下的高度是否小于MaxH,这个判断基本就是为了刚开始跳下和跳到地面而设立的.`坑点:跳到第一块板就brake了,因为不可能越过这块板跳到下一块`


    #include <stdio.h>
    #include <algorithm>
    #include <string.h>

    using namespace std;

    struct node{
        int x1, x2, h;
    }arr[1005];

    int t, n, x, y, maxH;
    int dp[1005][2];

    int cmp(node a, node b){
        return a.h < b.h;
    }

    int dfs(int d, int x, int idx){
        if (!dp[idx][d]){
            int minTime = 0x3f3f3f3f;
            int isJump = 1;
            for (int i = idx - 1; i >= 0; --i) {
                if (arr[idx].h - maxH > arr[i].h) break;
                if (x >= arr[i].x1 && x <= arr[i].x2){
                    minTime = min(minTime, dfs(0,arr[i].x1, i) + abs(x - arr[i].x1) + arr[idx].h - arr[i].h);
                    minTime = min(minTime, dfs(1,arr[i].x2, i) + abs(arr[i].x2 - x) + arr[idx].h - arr[i].h);
                    isJump = 0;
                    break;
                }
            }
            if (isJump && arr[idx].h <= maxH) dp[idx][d] = arr[idx].h;
            else dp[idx][d] = minTime;
        }
        return dp[idx][d];
    }

    int main(){
        scanf("%d", &t);
        for (int i = 0; i < t; ++i) {
            scanf("%d %d %d %d", &n, &x, &y, &maxH);
            arr[0].x1 = arr[0].x2 = x;
            arr[0].h = y;
            fill(dp[0], dp[0] + 1005 * 2, 0);
            for (int j = 1; j < n + 1; ++j) {
                scanf("%d %d %d", &arr[j].x1, &arr[j].x2, &arr[j].h);
            }
            sort(arr, arr + n + 1, cmp);
            printf("%d\n", dfs(1, x, n));
        }
        return 0;
    }
