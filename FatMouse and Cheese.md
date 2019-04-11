[FatMouse and Cheese](https://vjudge.net/contest/293713#problem/P)

##### 记忆化搜索,类似于dfs,不断向后搜索,然后由后面return到前.

    #include <stdio.h>
    #include <string.h>
    #include <queue>

    using namespace std;

    struct node {
        int x, y;
    };

    int n, k;
    int map[105][105];
    int dp[105][105];

    int dfs(node a) {
        if (!dp[a.x][a.y]) {
            int maxv = 0;
            for (int x = max(0, a.x - k); x < min(n, a.x + k + 1); ++x) {
                if (map[x][a.y] > map[a.x][a.y]) {
                    maxv = max(dfs(node{x,a.y}), maxv);
                }
            }
            for (int y = max(0, a.y - k); y < min(n, a.y + k + 1); ++y) {
                if (map[a.x][y] > map[a.x][a.y]) {
                    maxv = max(dfs(node{a.x,y}), maxv);
                }
            }
            dp[a.x][a.y] = maxv + map[a.x][a.y];
        }
        return dp[a.x][a.y];
    }

    int main() {
        while (scanf("%d %d", &n, &k)) {
            if (n == k && k == -1) break;
            for (int i = 0; i < n; ++i) {
                for (int j = 0; j < n; ++j) {
                    scanf("%d", &map[i][j]);
                }
            }
            memset(dp, 0, sizeof(dp));
            printf("%d\n", dfs(node{0, 0}));
        }
        return 0;
    }
