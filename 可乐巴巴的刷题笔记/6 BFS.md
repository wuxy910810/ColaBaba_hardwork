寻找最短路径，队列，遍历
***
# 0 BFS核心思想
**广度优先搜索算法**：把问题抽象成图，从点开始，向四周扩散。**可使用队列，每次把一个节点周围的所有节点加入队列**，一次一步，齐头并进。

**数据结构**：队列 queue

**BFS和DFS的区别**:BFS的路径一定最短。但是 空间复杂度高，以空间换时间。DFS是点，BFS是面。
**记录层数的方法**：
1. 每while一次，cnt++，for(int i = 0; i < que.size(); i++)循环队列中的所有元素。  
2. 使用其他方法，如所有元素的一个map, 初始化为0。从当前元素a搜索到下一个元素b，则mp[b] = mp[a]++;  push的时候，再cnt = mp[b];

# 1 例子
**1.【二叉树的最小深度】**

给定一个二叉树，找出其最小深度。
最小深度是从根节点到最近叶子节点的最短路径上的节点数量。

说明：叶子节点是指没有子节点的节点。
```cpp
int minDepth(TreeNode* root) {
    if (root == nullptr) {
        return 0;
    }
    queue<pair<TreeNode *, int>> que;
    que.emplace(root, 1);
    while (!que.empty()) {
        TreeNode *node = que.front().first;
        int depth = que.front().second;
        que.pop();
        /* 判断终止搜索的条件是否达到 */
        if (node->left == nullptr && node->right == nullptr) {
            return depth;
        }
        /* 继续往下搜索，并更新搜索内容 */
        if (node->left != nullptr) {
            que.emplace(node->left, depth + 1);
        }
        if (node->right != nullptr) {
            que.emplace(node->right, depth + 1);
        }
    }
    return 0;
}
```
**2. 【打开转盘锁】**
你有一个带有四个圆形拨轮的转盘锁。每个拨轮都有10个数字： '0', '1', '2', '3', '4', '5', '6', '7', '8', '9' 。每个拨轮可以自由旋转：例如把 '9' 变为  '0'，'0' 变为 '9' 。每次旋转都只能旋转一个拨轮的一位数字。

锁的初始数字为 '0000' ，一个代表四个拨轮的数字的字符串。

列表 deadends 包含了一组死亡数字，一旦拨轮的数字和列表里的任何一个元素相同，这个锁将会被永久锁定，无法再被旋转。

字符串 target 代表可以解锁的数字，你需要给出最小的旋转次数，如果无论如何不能解锁，返回 -1。

```cpp
class Solution {
public:
    int openLock(vector<string>& deadends, string target) {
        set<string> dead(deadends.begin(), deadends.end());
        set<string> visited;
        queue<pair<string, int>> que;
        
        que.emplace("0000", 0);
        while(!que.empty()) {
            string cur = que.front().first;
            int steps = que.front().second;
            que.pop();
            if (dead.count(cur) != 0) { 
                continue;
            }
            /* 判断终止搜索的条件是否达到 */
            if (cur == target) {
                return steps;
            }
            /* 继续往下搜索，并更新搜索内容 */
            for (int j = 0; j < 4; j++) {
                string up = plusOne(cur, j);
                if (!visited.count(up)) {
                    que.emplace(up, steps + 1);
                    visited.insert(up);
                }    
                string down = minusOne(cur, j);
                if (!visited.count(down)) {
                    que.emplace(down, steps + 1);
                    visited.insert(down);
                }    
            }
        }
        return -1;
    }
private:
    string plusOne(string cur, int j) {
        string res = cur;
        res[j] = ((res[j] - '0' + 1) % 10) + '0';
        return res;
    }

    string minusOne(string cur, int j) {
        string res = cur;
        res[j] = ((res[j] - '0' - 1 + 10) % 10) + '0';
        return res;
    }

};
```