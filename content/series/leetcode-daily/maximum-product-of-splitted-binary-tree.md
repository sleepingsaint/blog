---
title: "Maximum Product of Splitted Binary Tree"
date: 2021-08-19T20:02:40+05:30
draft: false 
tags: ["binary tree", "recursion"]
series: "leetcode-daily" 
---

## Question

Given the root of a binary tree, split the binary tree into two subtrees by removing one edge such that the product of the sums of the subtrees is maximized.

Return the maximum product of the sums of the two subtrees. Since the answer may be too large, return it modulo 109 + 7.

Note that you need to maximize the answer before taking the mod and not after taking it.

## Examples

Input: root = [1,2,3,4,5,6] <br/>
Output: 110 <br/>
Explanation: Remove the red edge and get 2 binary trees with sum 11 and 10. Their product is 110 (11*10)
<hr />

Input: root = [1,null,2,3,4,null,null,5,6] <br />
Output: 90 <br />
Explanation: Remove the red edge and get 2 binary trees with sum 15 and 6.Their product is 90 (15*6)
<hr />

Input: root = [2,3,9,10,7,8,6,5,4,11,1] <br />
Output: 1025
<hr />

Input: root = [1,1] <br />
Output: 1

## Constraints

* The number of nodes in the tree is in the range [2, 5 * 104].
* 1 <= Node.val <= 104

## Explanation

So the logic to solve this question is very simple which is (sum of subtree 1) * (sum of subtree 2). But how to calculate the sum of subtrees. We can do a __dfs__ to calculate the sum and then we __recursively__ calculate the answer by splitting at each node.

To avoid calculating the sum for each subtree we maintain a variable called total which holds the total sum of the tree, with this we can get the sum of other subtree, if we know the sum of another subtree.

## Code

```cpp

	#define mod 1000000007
	typedef long long ll;

	class Solution {
	public:
		
		int maxProduct(TreeNode* root) {
			if(!root) return 0;
			ll total = (ll) getSum(root);
			
			return dfs(root, total) % mod;
		}
		
		// recursively computing the answer
		long long dfs(TreeNode* root, long long total){
			if(!root) return 0;
			
			ll ans = 0, rsum = 0, lsum = 0;
			
			// if we can split in the left subtree
			// we split and check the answer
			if(root->left){
				ll left = (ll) root->left->val;
				rsum = (ll) total - left;
				ans = max(ans, left * rsum);

				// recursively calling in the left subtree
				ans = max(ans, dfs(root->left, total));
			}
			
			// if we can split in the right subtree
			// we split and check answer
			if(root->right){
				ll right = root->right->val;
				lsum = (ll) total - right;
				ans = max(ans, right * lsum);
				
				// recursively calling the right subtree
				ans = max(ans, dfs(root->right, total));
			}
			
			return ans;
		}
		
		// calculating the sum of the subtree with a given root
		// after calculating we are updating the root val with the subtree sum
		int getSum(TreeNode* root){
			if(!root) return 0;
			return root->val = root->val + getSum(root->left) + getSum(root->right);
		}
	};

```

## Space and Time Complexity

N - number of nodes in the tree

* Space Complexity: O(logN) - for the recursion stack the maximum depth will be max height of the tree
* Time Complexity: O(2 * N) - we are visiting every twice, once while computing the sum and the second time while computing the answer