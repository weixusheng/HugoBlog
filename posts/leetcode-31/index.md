# Leetcode Part-31


DP-64.最小路径和
<!--more-->

好久没做动态规划了!好经典动态规划题
```c++
class Solution {
public:
    int minPathSum(vector<vector<int>>& grid) {
        if(grid.size() == 0 || grid[0].size() == 0){
            return 0;
        }
        int row = grid.size();
        int colume = grid[0].size();
        auto dp = vector<vector<int> >(row,vector<int>(colume));
        dp[0][0] = grid[0][0];
        for(int i=1; i<row; i++){
            dp[i][0] = grid[i][0] + dp[i-1][0];
        }
        for(int j=1; j<colume; j++){
            dp[0][j] = grid[0][j] + dp[0][j-1];
        }
        for(int i=1; i<row; i++){
            for(int j=1; j<colume; j++){
                dp[i][j] = min(dp[i-1][j],dp[i][j-1]) + grid[i][j];
            }
        }
        return dp[row-1][colume-1];
    }
};
```
