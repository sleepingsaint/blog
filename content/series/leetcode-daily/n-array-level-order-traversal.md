---
title: "N Array Level Order Traversal"
date: 2021-08-06T12:59:53+05:30
draft: false 
tags: ["tree", "bfs", "queue"]
series: "leetcode-daily" 
---

## Question

Given an n-ary tree, return the level order traversal of its nodes' values.

Nary-Tree input serialization is represented in their level order traversal, each group of children is separated by the null value (See examples).

## Examples

* Input: root = [1,null,3,2,4,null,5,6]
* Output: [[1],[3,2,4],[5,6]]
<hr />

* Input: root = [1,null,2,3,4,5,null,null,6,7,null,8,null,9,10,null,null,11,null,12,null,13,null,null,14]
* Output: [[1],[2,3,4,5],[6,7,8,9,10],[11,12,13],[14]]

## Constraints

* The height of the n-ary tree is less than or equal to 1000
* The total number of nodes is between [0, 104]

## Explanation

### Level Order Traversal
As the name suggests we travese the tree level wise, first the root, then all the children of the root, then children of children of the root and so on.

This question is extension of level order traversal of binary tree. Here instead of two children we can have multiple children, so we can use a for loop to iterate through the children.

Since this is a __bfs__ approach we can use __queue__ data structure to do the traversal. We first insert the root in the queue and then iterate over the root children and so on. 

## Code

```cpp

	class Solution {
	public:
		vector<vector<int>> leve<hr />lOrder(Node* root) {
			vector<vector<int>> ans;
			if(!root) return ans;
			
			queue<Node*> q;
			q.push(root);
			
			while(!q.empty()){
				vector<int> tmp;
				int size = q.size();
				for(int i = 0; i < size; i++){
					Node* node = q.front();
					q.pop();
					tmp.push_back(node->val);
					
					for(Node* child: node->children)
						q.push(child);
				}
				ans.push_back(tmp);
			}
			
			return ans;
		}
	};

```

## Space and Time Complexity

* Space - O(N)
* Time - O(N)

where N is total number of nodes in the tree