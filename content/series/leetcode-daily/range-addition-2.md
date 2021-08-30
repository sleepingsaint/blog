---
title: "Range Addition 2"
date: 2021-08-30T19:19:25+05:30
draft: false 
tags: ["array", "math"]
series: "leetcode-daily" 
---

## Question

You are given an m x n matrix M initialized with all 0's and an array of operations ops, where ops[i] = [ai, bi] means M[x][y] should be incremented by one for all 0 <= x < ai and 0 <= y < bi.

Count and return the number of maximum integers in the matrix after performing all the operations.

## Examples

<img src="example1.jpg" />

Input: m = 3, n = 3, ops = [[2,2],[3,3]] <br />
Output: 4 <br />
Explanation: The maximum integer in M is 2, and there are four of it in M. So return 4.
<hr />

Input: m = 3, n = 3, ops = [[2,2],[3,3],[3,3],[3,3],[2,2],[3,3],[3,3],[3,3],[2,2],[3,3],[3,3],[3,3]] <br />
Output: 4
<hr />

Input: m = 3, n = 3, ops = [] <br />
Output: 9

## Constraints

* 1 <= m, n <= 4 * 10^4
* 1 <= ops.length <= 10^4
* ops[i].length == 2
* 1 <= ai <= m
* 1 <= bi <= n

## Explanation

A brute force approach would be to mark the matrix for every operation and then find the occurences of the maximum integers. Given the constraints it will give TLE for sure. 

If you observe carefully, we can notice that every answer, ie., the matrix which contains maximum integer always starts at (0, 0) position. What I want to explain is that for every operation we are modifying some portion of the matrix starting from the first position [0, 0].

For example consider the first example, 

* First Operation: [2, 2] - we can increase all the number is in the given range
* Second Operation: [3, 3] - while this operation also updates the values inside the [2, 2] range

lets say there is also another operation, say [1, 1]. After performing operation, some part of [2, 2] range gets updated and the max value will be present in the range [1, 1]. So if we perform any operation, we range with minimum range values on both axes gets updated and that range contains the answer.

We can also notice one thing, which is if there are no ops to perform then all the values in the matrix which are zeros will be largest numbers hence the answer would be number of zeros in the matrix.

So we calculate the minimum of x, y axes ranges and return the number of values in that range [x, y]

## Code

```cpp

	class Solution {
	public:
		int maxCount(int m, int n, vector<vector<int>>& ops) {
			// if there are no ops then all the values / zeros
			// in the matrix will be the largest numbers
			// hence the answer will be number of cells in the matrix
			if(ops.empty())
				return m*n;

			// calculating the minimum range values in both x, y axis
			for(vector<int> &op: ops){
				m = min(m, op[0]);
				n = min(n, op[1]);
			}
			
			return m*n;
		}
	};

```

## Space and Time Complexity

N - number of ops

* Space Complexiy: O(1) - No matter the N, we don't use any extra variables, hence constant space complexity
* Time Complexity: O(N) - for calculating the minimum values in the x, y axes.