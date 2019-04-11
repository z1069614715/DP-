[piggyBank](https://vjudge.net/contest/293713#problem/F)

##### 其中一个注意点就是dp[0] = 0!!!
##### 这是完全背包问题,但是是求最少能装多少,需要注意的是k必须从小到大,不然全是0x3f3f3f3f,原因就是要从容量为0开始dp.

    #include <stdio.h>
    #include <string.h>
    #include <algorithm>

    using namespace std;

    int T, E, F, N;
    int coin[505], weight[505];
    int dp[10010];

    int main(){
        scanf("%d", &T);
        for (int i = 0; i < T; ++i) {
            memset(coin, 0, sizeof(coin));
            memset(weight, 0, sizeof(weight));
            memset(dp, 0x3f, sizeof(dp));
            scanf("%d %d %d", &E, &F, &N);
            for (int j = 0; j < N; ++j) {
                scanf("%d %d", &coin[j], &weight[j]);
            }
            if (F == E){
                printf("The minimum amount of money in the piggy-bank is 0.\n");
                continue;
            }
            dp[0] = 0;
            for (int j = 0; j < N; ++j) {
                for (int k = weight[j]; k <= F - E; ++k) {
                    dp[k] = min(dp[k], dp[k - weight[j]] + coin[j]);
                }
            }
            if (dp[F - E] < 0x3f3f3f3f) printf("The minimum amount of money in the piggy-bank is %d.\n", dp[F - E]);
            else printf("This is impossible.\n");
        }
        return 0;
    }
