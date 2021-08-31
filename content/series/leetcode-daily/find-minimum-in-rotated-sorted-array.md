---
title: "Find Minimum in Rotated Sorted Array"
date: 2021-08-31T14:02:23+05:30
draft: false 
tags: ["array", "binary search"]
series: "leetcode-daily" 
---

## Question

Suppose an array of length n sorted in ascending order is rotated between 1 and n times. For example, the array nums = [0,1,2,4,5,6,7] might become:

[4,5,6,7,0,1,2] if it was rotated 4 times.
[0,1,2,4,5,6,7] if it was rotated 7 times.
Notice that rotating an array [a[0], a[1], a[2], ..., a[n-1]] 1 time results in the array [a[n-1], a[0], a[1], a[2], ..., a[n-2]].

Given the sorted rotated array nums of unique elements, return the minimum element of this array.

You must write an algorithm that runs in O(log n) time.

## Examples

Input: nums = [3,4,5,1,2]
Output: 1
Explanation: The original array was [1,2,3,4,5] rotated 3 times.
<hr />

Input: nums = [4,5,6,7,0,1,2]
Output: 0
Explanation: The original array was [0,1,2,4,5,6,7] and it was rotated 4 times.
<hr />

Input: nums = [11,13,15,17]
Output: 11
Explanation: The original array was [11,13,15,17] and it was rotated 4 times. 

## Constraints

* n == nums.length
* 1 <= n <= 5000
* -5000 <= nums[i] <= 5000
* All the integers of nums are unique.
* nums is sorted and rotated between 1 and n times.

## Explanation

When we see the time complexity O(logN) the first thing which pops in our brain is __binary search__. So we proceed with that, if we consider the basic case, where the array is in increasing sorted order (may be after rotations or may be not rotated), then we continue with normal basic binary search, but now if the array is rotated, we need to define conditions to move either to left or right side of the partition. 

Lets say the array is rotated, then one of the two cases should happen:

start, end - indicates the endpoints of the binary search range

* An element where, element < start, which means, there exists larger numbers in the left region and there might be minimum number in the left region, so we move left to find the minimum element
* An element where element > end, which means, there exists larger numbers in the left region, so we move right.

## Code

```cpp

	class Solution {
	public:
		int findMin(vector<int>& nums) {
			int n = nums.size();
			int start = 0, end = n - 1;
			while(start < end){
				int mid = start + (end - start) / 2;

				// if element < start, we move left
				if(nums[mid] < nums[start]){
					end = mid;
				}else if(nums[mid] > nums[end]){
					// if element > end, we move right
					start = mid + 1;
				}else end = mid; // normal binary search
			}
			
			return nums[start];
		}
	};

```

## Space and Time Complexity

N - number of elements in the given array

* Space Complexity: O(1) - no matter the length of the input, we use only 3 variables, so constant space complexity
* Time Complexity: O(logN) - for binary search