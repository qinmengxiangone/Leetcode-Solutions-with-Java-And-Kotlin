题目来源于 `LeetCode` 剑指 `offer` 第 `28` 号问题：对称的二叉树。和第 [101 号题](https://leetcode-cn.com/problems/symmetric-tree/) 相同。

* [中文地址：https://leetcode-cn.com/problems/dui-cheng-de-er-cha-shu-lcof](https://leetcode-cn.com/problems/dui-cheng-de-er-cha-shu-lcof)

## 题目描述

请实现一个函数，用来判断一棵二叉树是不是对称的。如果一棵二叉树和它的镜像一样，那么它是对称的。

例如，二叉树 [1,2,2,3,4,4,3] 是对称的。

```
    1
   / \
  2   2
 / \ / \
3  4 4  3
```

但是下面这个 [1,2,2,null,3,null,3] 则不是镜像对称的:

```
    1
   / \
  2   2
   \   \
   3    3
```

**示例 1：**

```
输入：root = [1,2,2,3,4,4,3]
输出：true
```

**示例 2：**

```
输入：root = [1,2,2,null,3,null,3]
输出：false
```

**限制：**

```
0 <= 节点个数 <= 1000
```

## 思路一：前序遍历变形

**算法流程如下：**

假设二叉树的左右节点为 L 和 R

一颗对称的二叉树，一定满足以下特点

* `L.val == R.val`，当前的对称节点是相等的
* `L.left.val, R.right.val`，L 的左子树的节点 和 R 的右子树的节点是相等的
* `L.right.val, R.left.val`，L 的右子树的节点 和 R 的左子树的节点是相等的

根据以上特点按照二叉树的前序遍历，判断是否是对称的二叉树

**复杂度分析：**

* 时间复杂度 `O(N)` ，其中 `N` 为二叉树的节点数量
* 空间复杂度 `O(N)` ， 最差情况下（当二叉树退化为链表）

### Kotlin 同 Java 的实现

```
class Solution {
    public boolean isSymmetric(TreeNode root) {
        if (root == null) return true;
        return symmetric(root.left, root.right);
    }

    public boolean symmetric(TreeNode L, TreeNode R) {
        if (L == null && R == null) return true;
        if (L == null || R == null || L.val != R.val) return false;

        return symmetric(L.left, R.right) && symmetric(L.right, R.left);
    }
}
```

## 思路二：镜像对称

一颗对称的二叉树，一定是镜像对称，步骤如下所示：

* 生成二叉树的镜像：利用后续遍历，交换左右子树即可
* 根据先序遍历，判断镜像二叉树和原二叉树是否相等

### Java 同 Kotlin 实现

```
class Solution {
    fun isSymmetric(root: TreeNode?): Boolean {
        if (root == null) return true
        val mirror = mirrorTree(root)
        return isSame(mirror, root)
    }

    /**
     * 根据先序遍历，判断镜像二叉树和原二叉树是否相等
     */
    fun isSame(a: TreeNode?, b: TreeNode?): Boolean {
        if (a == null && b == null) return true
        if (a == null || b == null || a.`val` != b.`val`) return false
        return return isSame(a.left, b.left) && isSame(a.right, b.right)
    }

    /**
     * 生成二叉树的镜像
     */
    fun mirrorTree(root: TreeNode?): TreeNode? {
        if (root == null) return root
        val node = TreeNode(root.`val`)
        node.left = mirrorTree(root.right)
        node.right = mirrorTree(root.left)
        return node
    }
}
```


