---
title: "Unique Binary Search Trees 2"
date: 2021-09-02T20:30:38+05:30
draft: false 
tags: ["binary search tree", "recursion"]
series: "leetcode-daily" 
---

## Question

Given an integer n, return all the structurally unique BST's (binary search trees), which has exactly n nodes of unique values from 1 to n. Return the answer in any order.

## Examples

<img src="example1.jpg" />

Input: n = 3 <br />
Output: [[1,null,2,null,3],[1,null,3,2],[2,1,3],[3,1,null,null,2],[3,2,null,1]]
<hr />

Input: n = 1 <br />
Output: [[1]]

## Constraints

* 1 <= n <= 8

## Explanation

To construct any binary tree or binary search tree, we need root, left and right subtrees. So in the problem, if we consider one number in the given range as root, then elements to the left of it will be part of left substree and similarly for right subtree.

So how do we calculate this left and right subtree, we use __recursion__. We recursively calculate the left and right subtrees and form trees with the current root.

If we have l left unique subtrees, r right unique subtrees then total number of trees with given root is l*r. 
If l is 0, then number of trees will be r, else if r is 0, then number of trees will be l. 

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
		vector<TreeNode*> generateTrees(int n) {
			// calling the recursive function
			// to construct unique tree in range [1, n]
			return solve(1, n);
		}
		
		vector<TreeNode*> solve(int start, int end){
			vector<TreeNode*> res;

			// if the range contains only one element
			// then it is a leaf node, so no need to calculate the subtrees. 
			if(start == end){
				TreeNode* root = new TreeNode(start);
				res.push_back(root);
				return res;
			}
			
			for(int i = start; i <= end; i++){
			
				// variables to store the left and right subtrees
				vector<TreeNode*> left, right;
				
				// calling the recursive function
				// to calculate the left and right subtrees
				if(i != start)
					left = solve(start, i - 1);
				if(i != end)
					right = solve(i + 1, end);
				

				vector<TreeNode*> tmp;

				// if left subtree is empty 
				// the trees will be root + right subtrees
				if(left.empty()){
					for(TreeNode* &node: right){
						TreeNode* root = new TreeNode(i);
						root->right = node;
						tmp.push_back(root);
					}	
				}
				
				// if right subtree is empty 
				// the trees will be left + root subtrees
				else if(right.empty()){
					for(TreeNode* &node: left){
						TreeNode* root = new TreeNode(i);
						root->left = node;
						tmp.push_back(root);
					}
				}
				
				// if both subtrees are non empty
				// then total trees will be left*right
				else {
					for(TreeNode* &lnode: left){
						for(TreeNode* &rnode: right){
							TreeNode* root = new TreeNode(i);
							root->left = lnode;
							root->right = rnode;
							
							tmp.push_back(root);
						}
					}
				}
				
				// adding the current trees to final result
				if(!tmp.empty()){
					res.insert(res.end(), tmp.begin(), tmp.end());
				}
			}
			
			return res;
		}
	};

```

## Space and Time Complexity

* Space Complexity: O(n) - in worst case the depth of recursion stack will be n, while considering the starting element as a root.
* Time Complexity: O(n^2) - since every node is visited n times, once for the outer for loop and and other while constructing tree of other elements 