---
title: "Two Sum"
date: 2021-08-02T12:51:20+05:30
draft: false 
tags: ["map", "unordered map"]
series: "leetcode-daily" 
---

## Question

Given an array of integers nums and an integer target, return indices of the two numbers such that they add up to target.

You may assume that each input would have __exactly one solution__, and you may not use the same element twice.

You can return the answer in any order.

## Examples

Input: nums = [2,7,11,15], target = 9
Output: [0,1]
Output: Because nums[0] + nums[1] == 9, we return [0, 1].

Input: nums = [3,2,4], target = 6
Output: [1,2]

Input: nums = [3,3], target = 6
Output: [0,1]

## Constraints

* 2 <= nums.length <= 104
* -109 <= nums[i] <= 109
* -109 <= target <= 109
* Only one valid answer exists.

## Explanation

A naive approach would be to run a nested for loop and check if the two elements add up to the target or not. If it gets added up return those two indices. Time complexity of this approach would be __O(n^2)__. 

For a better solution we can use __hashmap__ or __map or unordered map__ in C++. The idea behind this approach is once we select a particular element in the array we check in the hashmap if there is a counter part which adds up with the selected element to form the target. 

Since there might be duplicates in the array, we store array of indices as value for the key as the element in array.
   
## Code

```cpp
	
	class Solution {
	public:
    	vector<int> twoSum(vector<int>& nums, int target) {
				unordered_map<int, vector<int>> um;
				int n = nums.size();
				
				for(int i = 0; i < n; i++){
					um[nums[i]].push_back(i);    
				}
				
				for(int i = 0; i < n; i++){
					int k = target - nums[i];
					if(um[k].size() > 0){
						for(int j: um[k]){
							if(i != j)
								return vector<int>{i, j};
						}
					}
				}
				
				return vector<int>{-1, -1};
			}
		};
	}

```

## Space and Time Complexity

Space Complexity: O(n) 
Time Complexity: O(n) - single for loop and lookup in hashmap are of constant time
