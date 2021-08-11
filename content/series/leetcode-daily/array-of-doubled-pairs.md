---
title: "Array of Doubled Pairs"
date: 2021-08-11T19:53:05+05:30
draft: false 
tags: ["array", "hashmap"]
series: "leetcode-daily" 
---

## Question

Given an array of integers arr of even length, return true if and only if it is possible to reorder it such that arr[2 * i + 1] = 2 * arr[2 * i] for every 0 <= i < len(arr) / 2.

## Examples

Input: arr = [3,1,3,6]

Output: false
<hr />

Input: arr = [2,1,2,6]

Output: false
<hr />

Input: arr = [4,-2,2,-4]

Output: true

Explanation: We can take two groups, [-2,-4] and [2,4] to form [-2,-4,2,4] or [2,4,-2,-4].
<hr />

Input: arr = [1,2,4,16,8,4]

Output: false

## Constraints

* 0 <= arr.length <= 3 * 104
* arr.length is even.
* -105 <= arr[i] <= 105

## Explanation

The approach would be we sort the elements first and for every element we check if there exists a counter part i.e, 2*element, if it exists we remove both elements (curr element and its counter part) from the hashmap, if it doesn't exists we return false. 

But there is one case we need to handle and that is if the current element is a negative number. The problem with negative numbers is the counter parts (2*element) comes first in the sorted array, so for the negative numbers we also need to check if there exists a counter part ie, element / 2 and element is divisible by 2 because there might be different numbers which gives same result upon division, for example both -5, -4 gives -2 on division.

__But why sorting?__ 

Consider the case [4, 2, 1, 8], if we don't sort for the first iteration we take (4, 2) as one pair then we cannot pair the 1, 8 so we should return false, but in reality we should return true because we can form pairs like (1, 2), (4, 8).

If we sort the array then the case becomes [1, 2, 4, 8], so we can form pairs like (1, 2), (4, 8). 

Simply put, sorting guarentees the pairing of small number first, so that they don't get left behind. 

## Code

### Simple Approach

```cpp

	class Solution {
	public:
		bool canReorderDoubled(vector<int>& arr) {
			// sorting the elements
			sort(arr.begin(), arr.end());
			
			// storing the elements to check for the counter parts
			unordered_map<int, int> nums;
			for(int i: arr)
				nums[i]++;
			
			for(int i: arr){
				// if the element is already taken
				// then skip the iteration
				if(nums[i] == 0)
					continue;
				
				// for positive numbers find the 2*i counter part
				if(nums.find(2*i) != nums.end() && nums[2*i] != 0){
					nums[2*i]--;
					nums[i]--;   
				}
				// for negative numbers find the i / 2 counter part
				else if(i % 2 == 0 && nums.find(i / 2) != nums.end() && nums[i / 2] != 0){
					nums[i/2]--;
					nums[i]--;
				}
				// if we couldn't find the counter part
				// then return false because we cannot form the array
				else return false;
			}
			
			// if we reached this point that means we got the answer
			return true;
		}
	};

```

## Space and Time Complexity

* Space Complexity - O(N) - for storing all the elements in the hashmap
* Time Complexity O(2*N) - two for loops