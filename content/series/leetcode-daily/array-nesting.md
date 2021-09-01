---
title: "Array Nesting"
date: 2021-09-01T14:47:45+05:30
draft: false 
tags: ["array", "depth first search"]
series: "leetcode-daily" 
---

## Question

You are given an integer array nums of length n where nums is a permutation of the numbers in the range [0, n - 1].

You should build a set s[k] = {nums[k], nums[nums[k]], nums[nums[nums[k]]], ... } subjected to the following rule:

The first element in s[k] starts with the selection of the element nums[k] of index = k.
The next element in s[k] should be nums[nums[k]], and then nums[nums[nums[k]]], and so on.
We stop adding right before a duplicate element occurs in s[k].
Return the longest length of a set s[k].

## Examples

Input: nums = [5,4,0,3,1,6,2]<br />
Output: 4 <br />
Explanation: <br />
nums[0] = 5, nums[1] = 4, nums[2] = 0, nums[3] = 3, nums[4] = 1, nums[5] = 6, nums[6] = 2.<br />
One of the longest sets s[k]:<br />
s[0] = {nums[0], nums[5], nums[6], nums[2]} = {5, 6, 2, 0}
<hr />

Input: nums = [0,1,2]<br />
Output: 1

## Constraints

* 1 <= nums.length <= 10^5
* 0 <= nums[i] < nums.length
* All the values of nums are unique.

## Explanation

We can a brute force approach by taking every index and then constructing the set recursively, untill we run out of the elements or we encounter a already added element. This would give a __TLE__ given constraints of nums length.

We need a better approach. If we closly observe first example, if we start from the index 0, we reach 6, 2, 0, 5. Now after if we start from index 2 (element 0) or index 5 (element 6) or index 6 (element 2), we would get the same set. So we can reduce the redundant recursive calls by not checking if the element is already a part of set. Refer to the below image to better understand the explanation.

<img src="explanation.png" />

Source: LeetCode Solution Section

## Code

### Using Recusrion

```cpp

	class Solution {
	public:
		int arrayNesting(vector<int>& nums) {
			int n = nums.size();

			// boolean array to mark if the element 
			// is taken in any set or not
			bool *visited = new bool[n];
			for(int i = 0; i < n; i++)
				visited[i] = false;
			
			int count = 0;

			// variable to store the current set
			vector<int> curr;
			for(int i = 0; i < n; i++){
				// if the current element is not in any set 
				// we recursively call to compute its corresponding set
				if(!visited[i])
					solve(nums[i], nums, curr, visited, count);
			}
			
			return count;
		}
		
		// recursive function
		void solve(int root, vector<int> &nums, vector<int> &curr, bool *visited, int &ans){
			int c = curr.size(), n = nums.size();
			// if current element is already visited or
			// the current set contains all the elements in nums
			// update the final answer and return
			if(c == n || visited[root]){
				ans = max(ans, c);
				return;
			}
			
			// adding the element to the current set
			curr.push_back(root);

			// mark the element as visited
			visited[root] = true;
			
			// recursively calling the function
			solve(nums[root], nums, curr, visited, ans);
			curr.pop_back();
		}
	};

```

### Without recursion

```cpp

	class Solution {
	public:
		int arrayNesting(vector<int>& nums) {
			int n = nums.size();

			// boolean array to mark if the element 
			// is taken in any set or not
			bool *visited = new bool[n];
			for(int i = 0; i < n; i++)
				visited[i] = false;
			
			int count = 0;
			
			for(int i = 0; i < n; i++){
				// if the current element is not in any set 
				// we recursively call to compute its corresponding set
				if(!visited[i]){
					int curr = nums[i], tmpCount = 0;
					while(!visited[curr]){
						// marking the current element as picked
						visited[curr] = true;
						curr = nums[curr];

						// increasing the current set count
						tmpCount++;
					}

					// updating the answer
					count = max(count, tmpCount);
				}
			}
			
			return count;
		}
	};

```

## Space and Time Complexity

N - size of the nums array

* Space Complexity: O(N) - for storing the visited array
* Time Complexity: O(N) - we are iterating over every element only once