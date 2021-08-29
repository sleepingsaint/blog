---
title: "Patching Array"
date: 2021-08-29T23:10:49+05:30
draft: false 
tags: ["array", "greedy"]
series: "leetcode-daily" 
---

## Question

Given a sorted integer array nums and an integer n, add/patch elements to the array such that any number in the range [1, n] inclusive can be formed by the sum of some elements in the array.

Return the minimum number of patches required.

## Examples

Input: nums = [1,3], n = 6 <br />
Output: 1 <br />
Explanation:
Combinations of nums are [1], [3], [1,3], which form possible sums of: 1, 3, 4.
Now if we add/patch 2 to nums, the combinations are: [1], [2], [3], [1,3], [2,3], [1,2,3].
Possible sums are 1, 2, 3, 4, 5, 6, which now covers the range [1, 6].

So we only need 1 patch.
<hr />

Input: nums = [1,5,10], n = 20 <br />
Output: 2 <br />
Explanation: The two patches can be [2, 4].
<hr />

Input: nums = [1,2,2], n = 5 <br />
Output: 0

## Constraints

* 1 <= nums.length <= 1000
* 1 <= nums[i] <= 10^4
* nums is sorted in ascending order.
* 1 <= n <= 2^31 - 1

## Explanation

To solve this problem we need to add the required elements, so that we meet the given requirements. This is basically a extension of the [1789. Maximum Number of Consecutive Values You Can Make](https://leetcode.com/problems/maximum-number-of-consecutive-values-you-can-make/). 

So the basic intuition behind solving this problem is, if we already selected some coins and these coins can cover upto [1, k] range, then if the next coin is 

* __1__ we can select this coin so that the max range we can attain now increases to k + 1, i.e, [1, k + 1]
* __k + 1__, if we select this coin, that means we can make any number from 0 to k, so by just adding k to them, we can also make any number from k to k+(k+1) so, the range increases upto k + (k + 1), similarly for any coin which is less than k + 1.
* __k + 2__ or greater than __k + 2__, if we select this coin, we can never achieve the value k + 1, so any value greater than k + 1, we cannot pick.

So we sort all the given coins in increasing order, whenever we encounter the third case, then we __greedily__ add the k + 1 coin and continue our calculation.

## Code

```cpp

	class Solution {
	public:
		int minPatches(vector<int>& nums, int n) {
			int patches = 0, i = 0;

			// this holds the current maximum 
			// range we can cover with the given coins
			long long k = 0;
			
			while(i < nums.size() && k < n){
				// if we encounter the third case
				if(nums[i] > k + 1){
					// greedily adding the k + 1 element
					k += k + 1;

					// incrementing the number of patches done to the array
					patches += 1;
				}
				else {

					// for the first two cases we just add the 
					// current coin and keep procedding
					k += nums[i];
					i++;
				}
			}
			
			// there might be a case where we added all the coins
			// but still we didn't reach the target
			while(k < n){
				k += k + 1;
				patches += 1;
			}

			return patches;
		}
	};

```

## Space and Time Complexity

N - number of coins
T - target given

* Space Complexity: O(1) - It is constant because, no matter the length of the input we only 3 variables.
* Time Complexity: O(N + log(T - K)) - In the worst case, all the given coins are 1's and we need to iterate over the whole array and after that we need to add the next coin untill we reach the target, in this step the range is doubling (to be exact - k to 2k + 1), but for simplicity we can consider it to be doubled, so it will be log(T - K), where K is range after iterating over the given coins array.