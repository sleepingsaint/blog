---
title: "Count Good Nodes in Binary Tree"
date: 2021-08-17T12:58:18+05:30
draft: false 
tags: ["binary tree", "dfs"]
series: "leetcode-daily" 
---

## Question

Given a binary tree root, a node X in the tree is named good if in the path from root to X there are no nodes with a value greater than X.

Return the number of good nodes in the binary tree.

## Examples

Input: root = [3,1,4,3,null,1,5] <br/>
Output: 4 <br/>
Explanation: Nodes in blue are good. <br />
Root Node (3) is always a good node.<br /> 
Node 4 -> (3,4) is the maximum value in the path starting from the root.<br />
Node 5 -> (3,4,5) is the maximum value in the path<br />
Node 3 -> (3,1,3) is the maximum value in the path.

<hr />

Input: root = [3,3,null,4,2] <br />
Output: 3<br />
Explanation: Node 2 -> (3, 3, 2) is not good, because "3" is higher than it.

<hr />

Input: root = [1]<br />
Output: 1<br />
Explanation: Root is considered as good.

## Constraints

* The number of nodes in the binary tree is in the range [1, 10^5].
* Each node's value is between [-10^4, 10^4]

## Explanation

To solve this problem at any node, if we know the maximum value node in the path from root to the current node we can easily calculate the answer by comparing the current maximum value and current node value. After the comparision we try to update the maximum value and find the number of nodes in the left and right subtrees. This can be easily implemented using __recursion and dfs__.

## Code

```cpp

	class Solution {
	public:
		int goodNodes(TreeNode* root) {
			// if no root exists then good nodes are also 0
			if(!root) return 0;

			// calling the dfs function to calculate the good nodes in the tree
			return dfs(root, root->val);
		}
		
		int dfs(TreeNode* root, int maxVal){
			if(!root) return 0;
			int count = 0;

			// if current node value is greater than or equal
			// to current max value we increase the count
			if(root->val >= maxVal)
				count++;
			
			// updating the maxVal if current val is greater than curr max value
			maxVal = max(maxVal, root->val);

			// calculating the good nodes in left and right subtrees.
			count += dfs(root->left, maxVal);
			count += dfs(root->right, maxVal);

			return count;
		}
	};

```

## Space and Time Complexity

N - number of nodes in the tree

* Space Complexity - O(N) - in the worst case where tree is skewed so to store all the recursion stack we need O(N) space
* Time Complexity - O(N) - we are visiting every node only once