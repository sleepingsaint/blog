---
title: "Range Sum Query Immutable"
date: 2021-08-16T13:24:12+05:30
draft: false 
tags: ["array", "segment tree", "prefix sum"]
series: "leetcode-daily" 
---

## Question

Given an integer array nums, handle multiple queries of the following type:

Calculate the sum of the elements of nums between indices left and right inclusive where left <= right.

Implement the NumArray class:

* NumArray(int[] nums) Initializes the object with the integer array nums.
* int sumRange(int left, int right) Returns the sum of the elements of nums between indices left and right inclusive (i.e. nums[left] + nums[left + 1] + ... + nums[right]).

## Examples

Input

["NumArray", "sumRange", "sumRange", "sumRange"]

[[[-2, 0, 3, -5, 2, -1]], [0, 2], [2, 5], [0, 5]]

Output

[null, 1, -1, -3]
<div>
Explanation

NumArray numArray = new NumArray([-2, 0, 3, -5, 2, -1]);<br/>
numArray.sumRange(0, 2); // return (-2) + 0 + 3 = 1<br />
numArray.sumRange(2, 5); // return 3 + (-5) + 2 + (-1) = -1 <br />
numArray.sumRange(0, 5); // return (-2) + 0 + 3 + (-5) + 2 + (-1) = -3
<hr />

## Constraints

* 1 <= nums.length <= 104
* -105 <= nums[i] <= 105
* 0 <= left <= right < nums.length
* At most 104 calls will be made to sumRange.

## Explanation

I will explain three solutions here:
1. naive approach
2. Prefix Sums
3. Segment Tree

* Naive Approach

So the naive approach would be calculate the sum every time we are performing a query. This approach will run fine given the current constraints, but with larger constraints we might encounter TLE. 

* Prefix Sums

In this approach we precalculate the sums untill every index and when we need to do a query of range [i, j] we just return (sums till jth index - sums till (i - 1)th index). This is very intuitive and easy to understand. Problem with this approach is if the nums[i] is very large we might encounter overflow problems. In the question its mentioned __immutable__ so we need to maintain a extra array to store the result.

* Segment Tree

As the name suggests we divide the problem into smaller segments in this question we divide it into smaller ranges. The following image will give you the idea how we are going to solve this problem. As shown in the image we build the tree for multiple ranges and try to answer the given ranges by splitting them.

<div style="display: flex; justify-content: center">

![image](https://he-s3.s3.amazonaws.com/media/uploads/eec15d3.jpg)

</div>

For example, if we need to find the answer for range (2, 5), we can see that there is no (2, 5) range in the tree, so what we do is we split range to (2, 2) and (3, 5) which we have in the tree. We keep splitting the range untill we have a range in the tree.

Why segment tree?

Segment tree might seem complex to implement when compared to prefix sums, but learning how to implement this datastructure can help in other problems which involve functions other than summation, for example if this problems involves updation, in the prefix sum approach we need to update all the indexes after the current updated index which would be (in worst case) O(N), whereas in segment tree we need to do only log(N) updations.

## Code

### Naive approach

```cpp

	class NumArray {
	public:
		vector<int> nums;
		NumArray(vector<int>& nums) {
			this->nums = nums;
		}
		
		int sumRange(int left, int right) {
			if(left == right)
				return nums[left];
			
			// iterating over the range and finding the sum
			int count = 0;
			for(int i = left; i <= right; i++)
				count += nums[i];
			return count;
		}
	};

```

### Prefix Sums

```cpp

	class NumArray {
	public:
		vector<int> nums;
		NumArray(vector<int>& nums) {
			this->nums = nums;

			// computing the prefix sum array
			for(int i = 1; i < nums.size(); i++)
				this->nums[i] += this->nums[i - 1];
		}
		
		int sumRange(int left, int right) {
			// if left is 0 that means range is untill right index
			if(left == 0) return nums[right];
			return nums[right] - nums[left - 1];
		}
	};

```

### Segment Tree

```cpp

	// defining the segment tree node
	class Tree{
	public:
		// variables to store the value of the range
		// and left and right limits of the range
		int value, l, r;

		// variables to store the left and right children of the current node
		Tree *left, *right;
		
		Tree(int value, int l, int r, Tree* left, Tree* right){
			this->value = value;
			this->l = l;
			this->r = r;
			this->left = left;
			this->right = right;
		}
	};

	class NumArray {
	public:
		Tree* root;
		
		// building the segment tree
		Tree* buildSegmentTree(vector<int> &nums, int l, int r){
			// return NULL for invalid range
			if(l > r) return NULL;
			
			// if l == r that means we are looking at single 
			// element or in terms of tree languages, we are looking at a leaf node
			if(l == r){
				Tree* node = new Tree(nums[l], l, l, NULL, NULL);
				return node;
			}
			

			int mid = l + (r - l) / 2;
			// computing the left and right subtrees first
			Tree* left = buildSegmentTree(nums, l, mid);
			Tree* right = buildSegmentTree(nums, mid + 1, r);
			
			// building the current node with left and right subtrees
			Tree* node = new Tree(left->value + right->value, l, r, left, right);
			return node;
		}
		
		NumArray(vector<int>& nums) {
			root = buildSegmentTree(nums, 0, nums.size() - 1);
		}
		
		// get the value given the range
		int getValue(Tree* root, int l, int r){
			if(root->l == l && root->r == r) return root->value;
			int mid = root->l + (root->r - root->l) / 2;
			
			// if the range is completely inside the left child
			if(r <= mid) return getValue(root->left, l, r);

			// if the range is completely inside the right child
			if(l > mid) return getValue(root->right, l, r);
			
			// return value from the left and right child
			return getValue(root->left, l, mid) + getValue(root->right, mid + 1, r);
		}
		
		int sumRange(int left, int right) {
			return getValue(root, left, right);
		}
	};

```

## Space and Time Complexity
* Q - be number of queries
* N - size of nums array

Naive Approach:

* Space Complexity: O(N)
* Time Complexity: O(NxQ) - every time we iterating over the given range

Prefix Sum:

* Space Complexity: O(N) - for storing the nums array
* Time Complexity: O(N + Q) - for building the prefix sum array and O(1) for every query so for Q queries O(Q)

Segment Tree:

* Space Complexity: O(2xN - 1)
* Time Complexity: O(logN) - in the worst case we go to the depth of the tree which is logn

