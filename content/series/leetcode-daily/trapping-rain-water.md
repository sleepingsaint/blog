---
title: "Trapping Rain Water"
date: 2021-07-31T13:26:43+05:30
draft: false
series: "leetcode-daily"
tags: ["array"] 
---

## Question
Given n non-negative integers representing an elevation map where the width of each bar is 1, compute how much water it can trap after raining.

## Example

* Input: height = [0,1,0,2,1,0,1,3,2,1,2,1]
* Output: 6

<hr />

* Input: height = [4,2,0,3,2,5]
* Output: 9

## Constraints

Constraints:

* n == height.length
* 0 <= n <= 3 * 104
* 0 <= height[i] <= 105

## Explanation

On any point, to trap rain water we need to have walls adjacent to the current point with elevation more than the current elevation. In simple terms we need bigger walls than the current wall to trap the rain water. If we could get the value of the largest wall to the left and the right we check if we can trap the rain water on the current wall.

If the largest wall's to the left and right are equal to the current wall height, then we cannot store the water, else if the largest boundary wall's are greater then, we can store trap store of height __min(left largest wall height, right largest wall height) - current wall height__.  

```cpp

	class Solution {
	public:
		int trap(vector<int>& height) {
			int n = height.size();

			// if number of walls are less than or equal
			// to 2 we can't store the water
			if(n <= 2) return 0;
			
			// arrays to store the left and 
			// right largest walls
			int *left = new int[n];
			int *right = new int[n];
			
			// building the arrays
			left[0] = height[0];
			for(int i = 1; i < n; i++) left[i] = max(left[i - 1], height[i]);
			right[n - 1] = height[n - 1];
			for(int i = n - 2; i >= 0; i--) right[i] = max(right[i + 1], height[i]);
			
			int ans = 0;
			
			for(int i = 1; i < n - 1; i++){
				// calculating the amount of water can be trapped
				int h = min(left[i], right[i]);
				ans += h - height[i];
			}
			
			return ans;
		}
	};

```

## Space and Time Complexity

* Time Complexity: O(n)
* Space Complexity: O(2n)