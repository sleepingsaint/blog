---
title: "Flip String to Monotone Increasing"
date: 2021-08-11T09:01:38+05:30
draft: false 
tags: ["recursion", "dynamic programming", "array"]
series: "leetcode-daily" 
---

## Question

A binary string is monotone increasing if it consists of some number of 0's (possibly none), followed by some number of 1's (also possibly none).

You are given a binary string s. You can flip s[i] changing it from 0 to 1 or from 1 to 0.

Return the minimum number of flips to make s monotone increasing.

## Examples

Input: s = "00110"

Output: 1

Explanation: We flip the last digit to get 00111.
<hr />

Input: s = "010110"

Output: 2

Explanation: We flip to get 011111, or alternatively 000111.
<hr />

Input: s = "00011000"

Output: 2

Explanation: We flip to get 00000000.

## Constraints

* 1 <= s.length <= 105
* s[i] is either '0' or '1'.

## Explanation

So, to make a string monotone increasing, we can either all the 0's we found after a 1 to 1, or all the ones to 0. So this can be approached using recursion, where on every step we if we already had found 1 we flip the 0 and if we didn't found the 1 yet, we either flip the zero to 1 and make all the next 0's to 1 or if we find 1 we flip it to zero and continue the recursion. Look at the recursion code below to understand better.

As you can see the time complexity is exponential, because at recursion call we making 3 more recursion calls, so the time complexity will be O(3^n). 

A better approach would be to think it in terms of arrays. This might be a little bit tricky to come up with but once you understand it won't be anymore. 

So the approach would be to make string monotone increasing by making zeroes in range [0, x) and next ones in range [x, n]. So to do that we need to flip the ones in left range and zeroes in the second range. We can maintain a array to get the count of ones in a certain range, so that the time complexity reduces to O(n). 


## Code

### Recursion Code - O(3^n)

```cpp

	class Solution {
	public:
		int minFlipsMonoIncr(string s) {
			int n = s.length();
	        int i = 0;
	        while(i < n && s[i] == '0')
	            i++;
			
	        int ans = n;
	        dfs(s, i, false, 0, ans);
	        return ans;
		}
		
		void dfs(string s, int i, bool oneFound, int curr, int &ans){
			// here one found indicates if we have a 1 
			// in the previous positions of the string
			// if we have all the next characters 
			// should also be 1. if not we have four more cases

			int n = s.length();
			if(i == n){
				ans = min(ans, curr);
				return;
			}
			
			if(oneFound)
				// if we already have one in the previous positions
				// the next character should also be 1
				dfs(s, i + 1, oneFound, curr + (s[i] == '0'), ans);
			else{
				// in this case we don't have any ones prior
				if(s[i] == '1'){
					// we taking 1 into count
					dfs(s, i + 1, true, curr, ans);

					// flipping the 1 back to zero
					// since we don't have any ones before
					dfs(s, i + 1, false, curr + 1, ans);
				}else{
					// we are not flipping the 0 to 1
					dfs(s, i + 1, false, curr, ans);

					// flipping the 0 to 1
					dfs(s, i + 1, true, curr + 1, ans);
				}
			}
		}
	};

```

### Array Based Solution

```cpp

	class Solution {
	public:
		int minFlipsMonoIncr(string s) {
			int n = s.length();
			int ones[n];
			ones[0] = (s[0] == '1');

			// building the ones array
			for(int i = 1; i < n; i++){
				ones[i] = ones[i - 1] + (s[i] == '1' ? 1 : 0);
			}
			
			int ans = n;
			for(int i = 0; i <= n; i++){
				// we are making [0, x) as 0's
				// and the range [x, n) as 1's

				// calculating the ones in the [0, x) range
				int left_ones = (i == 0 ? 0 : ones[i - 1]);

				// calculating the zeroes in the [x, n) range
				int right_zeroes = i == n ? 0 : (n - i) - (ones[n - 1] - left_ones);
				ans = min(ans, right_zeroes + left_ones);
			}
			
			return ans;
		}
	}
```
## Space and Time Complexity

For Array based solution

* Space Complexity - O(N)
* Time Complexity - O(N)