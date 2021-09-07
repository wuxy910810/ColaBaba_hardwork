### 0. 概念

- DFS：深度优先搜索算法

解法1：递归

解法2：非递归，使用stack

- 递归是一种算法结构，回溯是一种算法思想。

回溯：通过不同的尝试，来生成问题的解。有点类似穷举，和穷举不同的是回溯会“剪枝”

回溯是DFS的一种。回溯在求解过程中不保留完整的数结构，而DFS记下完整的搜索树。

- 动态规划和回溯算法的异同：

动态规划的暴力求解阶段就是回溯算法。

动态规划有重叠子问题性质，可以使用dp table或者备忘录优化，进行剪枝。

回溯算法没有重叠子问题性质。

![img](https://pic.leetcode-cn.com/1611504979-HoeCGp-image.png)

### 1. DFS模板

``` c++
// dfs模板， 记忆化减枝
void dfs(路径，选择列表) {
    if (终止条件) {
        存放结果; // 若只需要一个解，只需要检查dfs的返回值，直接返回即可
        return;
    }

    for (选择：本层集合中元素（树中节点孩子的数量就是集合的大小）) { // 如上下左右等，(有时，需按照题意，构造选择的集合)
        // 排除不合法的选择;或者已经做过的选择
        // 做选择;同时将该选择从选择列表中移除
        dfs(路径，选择列表); // 递归
        // 回溯，撤销选择，将该选择再加入选择列表
    }
}
```
### 2.DFS模板

``` cpp
// eg. 93. 复原 IP 地址
class Solution {
public:
    vector<string> ans;
    string tmpString;
    vector<string> restoreIpAddresses(string s) {
        dfs(s, 0, 0);
        return ans;
    }

    void dfs(string &s, int index, int count)
    {
        if (count == 4 || s.size() == index) {
            if (count == 4 && s.size() == index) {
                ans.push_back(tmpString.substr(0, tmpString.size() - 1));
            }
            return;
        }

        for (int i = 1; i <= 3; i++) {
            // 排除不合法的选择选择
            if (index + i > s.size()) {
                continue;
            }
            if (s[index] == '0' && i != 1) {
                continue;
            }
            if (i == 3 && s.substr(index, i) > "255") {
                continue;
            }
            tmpString += s.substr(index, i);
            tmpString += ".";
            dfs(s, index + i, count + 1);
            // 撤销选择
            tmpString = tmpString.substr(0, tmpString.size() - i - 1);
        }
    }
};
```

