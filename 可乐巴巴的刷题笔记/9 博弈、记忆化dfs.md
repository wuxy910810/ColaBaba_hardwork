## 二进制位标记某个数是否被选择。（<32位的时候）

```cpp
state = state | (1 << i)
(state & (1 << i)) != 0 // 表示位标记
// 464. 我能赢吗？ https://leetcode-cn.com/problems/can-i-win/
// 记忆化dfs、博弈
class Solution {
public:
    map<pair<int, int>, bool> mp; // <state, sum>, ret
    bool canIWin(int maxChoosableInteger, int desiredTotal) {

        if ((1 + maxChoosableInteger) * maxChoosableInteger / 2 < desiredTotal) {
            return false;
        }
        for (int i = 1; i <= maxChoosableInteger; i++) {
            if (dfs(maxChoosableInteger, desiredTotal, 1 << i, i)) {
                return true;
            }
        }
        return false;
    }

    bool dfs(int maxChoosableInteger, int desiredTotal, int state, int sum)
    {
        if (sum >= desiredTotal) {
            return true;
        }
        if (mp.count({state, sum})) {
            return mp[{state, sum}];
        }
        bool ret = true;
        for (int i = 1; i <= maxChoosableInteger; i++) {
            if ((state & (1 << i)) != 0) {
                continue;
            }
            if (dfs(maxChoosableInteger, desiredTotal, (state | (1 << i)), sum + i) == true) {
                ret = false;  // 后手赢了，表示当前手输了，返回false
                break;
            }
        }
        mp[{state, sum}] = ret; 
        return ret;
    }
};
    
```

```cpp
// 486. 预测赢家 https://leetcode-cn.com/problems/predict-the-winner/
// 递归 
class Solution {
public:
    bool PredictTheWinner(vector<int>& nums) {
        // 递归
        return dfs(nums, 0, nums.size() - 1, 1) >= 0;
    }

    int dfs(vector<int>& nums, int left, int right, int flag)
    {
        if (left == right) {
            return nums[left] * flag;
        }

        int leftScore = dfs(nums, left + 1, right, -flag) + nums[left] * flag;
        int rightScore = dfs(nums, left, right - 1, -flag) + nums[right] * flag;
        return max(leftScore * flag, rightScore * flag) * flag;
    }
};

// 动态规划
bool PredictTheWinner(vector<int>& nums) {
    vector<vector<int>> dp(nums.size(), vector<int>(nums.size(), 0));
    for (int i = 0; i < nums.size(); i++) {
        dp[i][i] = nums[i];
    }
    // dp[i][j] = max(nums[i] - dp[i + 1][j], nums[j] - dp[i][j - 1])
    for (int i = nums.size() - 2; i >= 0; i--) {
        for (int j = i + 1; j < nums.size(); j++) {
            dp[i][j] = max(nums[i] - dp[i + 1][j], nums[j] - dp[i][j - 1]);
        }
    }
    return dp[0][nums.size() - 1] >= 0;
}
```

