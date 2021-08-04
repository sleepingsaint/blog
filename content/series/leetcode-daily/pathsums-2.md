---
title: "Pathsums 2"
date: 2021-08-04T13:06:06+05:30
draft: false 
tags: ["recursion", "binary tree", "tree"]
series: "leetcode-daily" 
---

## Question

Given the root of a binary tree and an integer targetSum, return all root-to-leaf paths where each path's sum equals targetSum.

A leaf is a node with no children.

## Examples

* Input: root = [5,4,8,11,null,13,4,7,2,null,null,5,1], targetSum = 22
* Output: [[5,4,11,2],[5,8,4,5]]

* Input: root = [1,2,3], targetSum = 5
* Output: []

* Input: root = [1,2], targetSum = 0
* Output: []

## Constraints

* The number of nodes in the tree is in the range [0, 5000].
* -1000 <= Node.val <= 1000
* -1000 <= targetSum <= 1000

## Explanation

Approach for this problem is straight forward. We go from the root to the leaf nodes and add the values along the path. After reaching the leaf node, if the total added value becomes equal to given target we add it to the answer. 

To iterate over the children nodes we can use the __recursion__. At every stage we remove the current node value from the target and check if we can form a path with current node as root and updated target (old target - current node value), if it can form a path we add it to the ans.

## Code

```cpp

	/**
	* Definition for a binary tree node.
	* struct TreeNode {
	*     int val;
	*     TreeNode *left;
	*     TreeNode *right;
	*     TreeNode() : val(0), left(nullptr), right(nullptr) {}
	*     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
	*     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
	* };
	*/
	class Solution {
	public:
		vector<vector<int>> pathSum(TreeNode* root, int targetSum) {
			vector<vector<int>> res;

			// if there is no root there is no path
			if(!root) return res;

			// checking at the leaf node
			if(!root->left && !root->right){
				if(root->val == targetSum){
					res.push_back(vector<int>{root->val});
				}
				return res;
			}
			
			// getting answers from the child nodes with target: targetSum - root->val 
			vector<vector<int>> left = pathSum(root->left, targetSum - root->val);
			vector<vector<int>> right = pathSum(root->right, targetSum - root->val);
			
			// if path exists we add the current root to the answers
			for(vector<int> &vec: left)
				vec.insert(vec.begin(), root->val);
			for(vector<int> &vec: right)
				vec.insert(vec.begin(), root->val);
			
			// add the children node answers to the current answer
			res.insert(res.end(), left.begin(), left.end());
			res.insert(res.end(), right.begin(), right.end());
			return res;
		}
	};

```

## Space and Time Complexity

N - total number of nodes 

* Time Complexity: O(N) - since we iterating every node only once.
* Space Complexity: O(N) - In the worst case scenario, we would be storing all the children nodes of the root. 