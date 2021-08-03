---
title: "Subsets 2"
date: 2021-08-03T13:33:12+05:30
draft: false
tags: ["recursion", "set", "map", "hashmap"]
series: "leetcode-daily"
---

## Question

Given an integer array nums that may contain duplicates, return all possible subsets (the power set).
The solution set must not contain duplicate subsets. Return the solution in any order.

## Examples

Input: nums = [1,2,2]
Output: [[],[1],[1,2],[1,2,2],[2],[2,2]]

Input: nums = [0]
Output: [[],[0]]

## Constraints

- 1 <= nums.length <= 10
- -10 <= nums[i] <= 10

## Explanation

This problem is similar to generating all the subsets but catch is to avoid duplicate subsets. So to check if a subset is already added to the answer, we can use a **hasmap or set**. We create a string representation of every subset and check if it already exists. If it doesn't, we add the string representation of subset to hashmap or set and the subset to the answer.

## Code

```cpp

	// In this code I used set to track the added the subsets
	typedef vector<vector<int>> vvi;

	class Solution {
		public:
			vector<vector<int>> subsetsWithDup(vector<int>& nums) {
				vvi ans;
				set<string> sets;
				sort(nums.begin(), nums.end());
				recursion(ans, sets, nums, 0, "", vector<int>{});
				return ans;
			}

			// recursive function to compute the subsets
			void recursion(vvi &ans, set<string> &sets, vector<int> &nums, int i, string curr, vector<int> vec){
				if(i == nums.size()){
					// checking if the sets already exists
					if(sets.find(curr) == sets.end()){

						// if subset doesn't exists
						// we add the subset to the ans
						// and string representation to set
						sets.insert(curr);
						ans.push_back(vec);
					}
					return;
				}

				recursion(ans, sets, nums, i + 1, curr, vec);
				vec.push_back(nums[i]);
				recursion(ans, sets, nums, i + 1, curr + to_string(nums[i]), vec);
			}
		};

```

## Space and Time Complexity

Time Complexity: O(2^n) - n is number of elements in nums array
Space Complexity: O(2^n) - In the worst case we store all the subsets string representations.
