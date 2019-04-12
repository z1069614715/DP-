[Help Jimmy](http://poj.org/problem?id=1661)



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
