---
title: "Verify Preorder Serialization of Binary Tree"
date: 2021-08-26T20:09:20+05:30
draft: false 
tags: ["recursion", "tree", "preorder traversal"]
series: "leetcode-daily" 
---

## Question

One way to serialize a binary tree is to use preorder traversal. When we encounter a non-null node, we record the node's value. If it is a null node, we record using a sentinel value such as '#'.

For example, the above binary tree can be serialized to the string "9,3,4,#,#,1,#,#,2,#,6,#,#", where '#' represents a null node.

Given a string of comma-separated values preorder, return true if it is a correct preorder traversal serialization of a binary tree.

It is guaranteed that each comma-separated value in the string must be either an integer or a character '#' representing null pointer.

You may assume that the input format is always valid.

For example, it could never contain two consecutive commas, such as "1,,3".

Note: You are not allowed to reconstruct the tree.

## Examples

<img src="example1.jpg" />

Input: preorder = "9,3,4,#,#,1,#,#,2,#,6,#,#" <br />
Output: true
<hr />

Input: preorder = "1,#" <br />
Output: false
<hr />

Input: preorder = "9,#,#,1" <br />
Output: false

## Constraints

* 1 <= preorder.length <= 104
* preoder consist of integers in the range [0, 100] and '#' separated by commas ','.

## Explanation

To solve this problem, we need to validate the given may be preorder traversal string. As per the question for every leaf node we need two #'s following the current character and also only root node, since a valid binary tree have only one root node.

At every node we recursively solve for the validilty of left and right subtrees.

## Code

```cpp

	class Solution {
	public:
		bool isValidSerialization(string preorder) {
			int index = 0;
			
			// converting the given string into vector of nodes
			vector<string> str;
			int start = 0;
			for(int i = 0; i < preorder.length(); i++){
				if(preorder[i] == ','){
					str.push_back(preorder.substr(start, i - start));
					start = i + 1;
				}
			}
			str.push_back(preorder.substr(start, preorder.length() - start));
			
			// calling the recursive function
			bool ans = solve(str, index);
			
			// we also check if all the nodes are visited or not
			// if not then there are more than 1 root node
			// which invalidates the tree
			return ans && index == str.size() - 1;
			
		}
		
		bool solve(vector<string> &preorder, int &i){
			int n = preorder.size();

			// if given root is leaf, then it is still valid
			if(preorder[i] == "#")
				return true;
			
			// if there are no subtrees for a node
			// i.e we have to have atleast two # as children
			// then return false
			if(i == n - 1)
				return false;
			
			bool ans = true;
			
			// solving for the left subtree
			ans &= solve(preorder, ++i);
			
			// if left subtree is invalid or 
			// right subtree doesn't exists return false
			if(!ans || i == n - 1)
				return false;
			
			// solving the right subtree
			ans &= solve(preorder, ++i);
			return ans;
		}
	};

```

## Space and Time Complexity

N - number of nodes in tree, (including the empty nodes)

* Space Complexity: O(N + N) - one N for storing the array of nodes, and another N for the recursive call, since we visit every node atleast once so max recursion depth will be N.
* Time Complexity: O(N + N) - one N for creating the array, and another N for at most N recursive calls

<br />