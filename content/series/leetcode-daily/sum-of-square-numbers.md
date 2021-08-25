---
title: "Sum of Square Numbers"
date: 2021-08-25T14:49:17+05:30
draft: false 
tags: ["Math"]
series: "leetcode-daily" 
---

## Question

Given a non-negative integer c, decide whether there're two integers a and b such that a^2 + b^2 = c.

## Examples

Input: c = 5 <br />
Output: true <br />
Explanation: 1 * 1 + 2 * 2 = 5
<hr />

Input: c = 3 <br />
Output: false
<hr />

Input: c = 4 <br />
Output: true
<hr />

## Constraints

* 0 <= c <= 231 - 1

## Explanation

The maximum value for a, b is square root of c, and the other value will be 0. So we need to check from 0 to square root of c and check if the remaining part is perfect square or not. The code below will give you better understanding.
 
## Code

```cpp

	class Solution {
	public:
		bool judgeSquareSum(int c) {
			int m = sqrt(c), n, k;

			// iterating from 0 to the square root of c
			for(int i = 0; i <= m; i++){
				n = c - (i * i);
				k = sqrt(n);

				// checking if the remaining part is a
				// perfect square or not. If is a perfect
				// square return true, else continue
				if(k*k == n)
					return true;
			}
			
			return false;
		}
	};

```

## Space and Time Complexity

* Space Complexity: O(1) - No matter the input we always have only three variables hence the constant complexity
* Time Complexity: O(sqrt(c)) - since we iterate till the square root of the c