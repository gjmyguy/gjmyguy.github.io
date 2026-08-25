## LeetCode-二叉树

#### 437.路径总和III

给定一个二叉树的根节点 `root` ，和一个整数 `targetSum` ，求该二叉树里节点值之和等于 `targetSum` 的 **路径** 的数目。

**路径** 不需要从根节点开始，也不需要在叶子节点结束，但是路径方向必须是向下的（只能从父节点到子节点）。

<img src="C:/Users/Kevin/AppData/Roaming/Typora/typora-user-images/image-20260625171650440.png" alt="image-20260625171650440" style="zoom:67%;" />

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def pathSum(self, root: Optional[TreeNode], targetSum: int) -> int:
        if root is None:
            return 0
        # 1. 以当前 root 为起点的路径数
        ans = self.rootSum(root, targetSum)
        
        # 2. 加上不以 root 为起点，而是以左孩子、右孩子为起点的路径数（递归整棵树）
        ans += self.pathSum(root.left, targetSum)
        ans += self.pathSum(root.right, targetSum)
        
        return ans
        
    def rootSum(self, root: Optional[TreeNode], targetSum: int) -> int:
        # 计算当前节点为起点的路径数量
        if root is None:
            return 0
        res = 0
        # 如果当前节点的值刚好等于需要的目标值，说明找到了一条路径
        if root.val == targetSum:
            res += 1
        # 继续向下寻找，目标值需要减去当前节点的值
        res += self.rootSum(root.left, targetSum-root.val)
        res += self.rootSum(root.right, targetSum-root.val)
        return res
        
```

**💡 核心解法：双重 DFS（递归）**

由于起点和终点都是任意的，我们可以将问题拆解为“双重任务”：

1. **第一重任务（主函数 `pathSum`）：** 负责**遍历整棵树**，让每一个节点都轮流当一次“路径的起点”。
2. **第二重任务（辅助函数 `rootSum`）：** 负责**固定起点向后搜索**。一旦起点固定，就向下延伸，利用 `targetSum - root.val` 动态减少目标值，直到正好减完（即 `root.val == targetSum`），说明找到一条合法的连续路径。

主函数 `pathSum` 的职责是遍历整棵树，通过递归调用自身，让树中的每一个节点都轮流作为一次“路径的绝对起点”，最终将所有节点作为起点时搜寻到的合法路径总数进行汇总。

辅助函数 `rootSum` 则负责执行具体的向下搜索任务，它被严格限制在“必须以当前节点为绝对起点向下延伸”。在向子节点推进的过程中，它通过 `targetSum - root.val` 动态减少目标值，当某一节点的数值刚好等于此时剩下的目标差值时，就意味着从最初起点到当前节点的整条连续路径刚好凑齐，触发计数加一；为了找出后面由于正负数抵消而产生的潜在路径，它在凑齐后依然会继续向下深挖。

以目标值 `targetSum = 8` 为例，当主函数遍历到节点 5 时，会启动 `rootSum(5, 8)`。搜寻向左遇到节点 3 时，因为之前的 5 已经消耗了部分目标值，剩余的目标差值刚好等于 3，满足 `3 == 3`，从而找到了 `5 -> 3` 这条路径；同理，搜寻向右经过 2 到达节点 1 时，剩余差值刚好等于 1，又揪出了 `5 -> 2 -> 1` 这条路径。随后主函数继续递归，当遍历到节点 -3 作为新起点时，`rootSum(-3, 8)` 向下延伸到 11，因 `-3 + 11 = 8` 再次触发计数，找到了 `-3 -> 11` 路径，最终 `pathSum` 汇总全树所有起点的贡献，得到总路径数为 3 条。