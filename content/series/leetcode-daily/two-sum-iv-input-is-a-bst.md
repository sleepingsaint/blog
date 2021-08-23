---
title: "Two Sum IV Input Is a Bst"
date: 2021-08-23T15:35:01+05:30
draft: false 
tags: ["binary search tree", "tree", "dfs", "hashmap"]
series: "leetcode-daily" 
---

## Question

Given the root of a Binary Search Tree and a target number k, return true if there exist two elements in the BST such that their sum is equal to the given target.

## Examples

Input: root = [5,3,6,2,4,null,7], k = 9 <br />
Output: true
<hr />

Input: root = [5,3,6,2,4,null,7], k = 28<br />
Output: false
<hr />

Input: root = [2,1,3], k = 4<br />
Output: true
<hr />

Input: root = [2,1,3], k = 1<br />
Output: false
<hr />

Input: root = [2,1,3], k = 3<br />
Output: true

## Constraints

* The number of nodes in the tree is in the range [1, 104].
* -104 <= Node.val <= 104
* root is guaranteed to be a valid binary search tree.
* -105 <= k <= 105

## Explanation

This problem is similar to any other two sum problem variant, but here we need to traverse a tree instead of an array. So one approach would be to first build the __hashmap__ and then at every node we check if the counter part exists or not. In this approach we need to traverse the tree two times. 

Another better approach would be to traverse the tree only once and compute the answer while traversing. We traverse the tree in a __dfs__ approach. At every node we check if the counter exists or not, if not we add the current root value to the hashmap.

## Code

### First Approach

```cpp

	class Solution {
	public:
		bool findTarget(TreeNode* root, int k) {
			map<int, int> count;
			// building the hashmap recursively
			buildCount(root, count);

			// for every element in the hashmap
			// or in other words every node in the tree
			for(auto it: count){
				int val = it.first;
				int rem = k - val;
				
				// decrementing because there might be duplicate values in bst
				// since its not clear in the question in that case we might get wrong
				// answer for some situtations

				// for example: k = 4, curr node = 2, count[2] = 1
				// in the above example if we don't decrement we would the answer 
				// as right but in reality we have only one 2 in the tree
				count[val]--;
				
				// checking the if the counter part exists
				if(count.find(rem) != count.end() && count[rem] > 0){
					return true;
				}
				count[val]++;
			}
			return false;
		}
		
		void buildCount(TreeNode* root, map<int, int> &count){
			if(!root) return;
			// add the root val to the hashmap
			count[root->val]++;

			// build the map for left and right subtrees
			buildCount(root->left, count);
			buildCount(root->right, count);
		}
	};

```

### Second (Efficient) Approach

```cpp

	class Solution {
	public:
		// hashmap to store the visited nodes
		unordered_map<int, int> m;
		
		bool findTarget(TreeNode* root, int k) {
			if(!root) return false;
			// checking if the counter part exists or not
			if(m.find(k - root->val) != m.end())
				return true;

			// adding the root value to the hashmap
			m[root->val]++;
			// recursively computing the answer
			return findTarget(root->left, k) || findTarget(root->right, k);
		}
	};

```

## Space and Time Complexity

N - total number of nodes in the tree

* Space Complexity: O(N) - for hashmap to store the nodes. In the worst case we find the answer in the last leaf node, so hashmap stores all the nodes
* Time Complexity: O(N) - In worst case the BST is a skewed tree so we visit every node atleast once.
