---
title: "Stone Game"
date: 2021-08-05T17:48:02+05:30
draft: false 
tags: ["dynamic programming", "recursion"]
series: "leetcode-daily" 
---

## Question

Alex and Lee play a game with piles of stones.  There are an even number of piles arranged in a row, and each pile has a positive integer number of stones piles[i].

The objective of the game is to end with the most stones.  The total number of stones is odd, so there are no ties.

Alex and Lee take turns, with Alex starting first.  Each turn, a player takes the entire pile of stones from either the beginning or the end of the row.  This continues until there are no more piles left, at which point the person with the most stones wins.

Assuming Alex and Lee play optimally, return True if and only if Alex wins the game.

## Examples

* Input: piles = [5,3,4,5]
* Output: true
* Explanation: 
Alex starts first, and can only take the first 5 or the last 5.
Say he takes the first 5, so that the row becomes [3, 4, 5].
If Lee takes 3, then the board is [4, 5], and Alex takes 5 to win with 10 points.
If Lee takes the last 5, then the board is [3, 4], and Alex takes 4 to win with 9 points.
This demonstrated that taking the first 5 was a winning move for Alex, so we return true.

## Constraints

* 2 <= piles.length <= 500
* piles.length is even.
* 1 <= piles[i] <= 500
* sum(piles) is odd.

## Explanation

The first idea which pops up when we see this kind of problems is __Greedy Approach__. In greedy approach, both players tries to pick the bigger pile since the goal of the game is to end up with more number of stones. But this approach will fail for some cases. But in this question if we consider a situation where alex always picks the larger pile and lee picks the smaller pile this approach works fine.

Next Approach would be to use __Recursion__. In every step, we pick elements from either ends, so there will be 4 cases. 
Consider we have piles from i to j. The four cases would be
* alex - ith, lee - (i + 1)th
* alex - ith, lee - jth
* alex - jth, lee - ith
* alex - jth, lee - (j - 1)th

This way we compute at every step. If we have no piles left we return if count of alex > count of lee.

We can improve the recursion approach using __Dynamic Programming__. We can store the answers of in (i, j) piles, if alex can win or not. In the recursion when ever we comes to point (i, j) we can decide whether to continue or not, base on

dp(i, j) - indicates if alex can win for case of (i, j) piles 
* If dp(i, j) is true, return true if (curr count of alex >= curr count of lee) else continue recursion
* If dp(i, j) is false, return false if(curr count of alex <= curr count of lee) else continue recursion

You can better understand the logic if you go through the code.

## Code

```cpp
	
	// greedy approach
	class Solution {
	public:
		vector<vector<int>> dp;
		
		bool stoneGame(vector<int>& piles) {
			int n = piles.size();
			
			int i = 0, j = n - 1;
			int alex = 0, lee = 0;
			
			while(i < j){
				if(piles[i] < piles[j])
					alex += piles[j--];
				else alex += piles[i++];
				
				if(piles[i] < piles[j])
					lee += piles[i++];
				else lee += piles[j--];
			}
			
			return (alex > lee);
		}
	};

```

```cpp

	// dynamic programming approach
	class Solution {
	public:
		vector<vector<int>> dp;
		
		bool stoneGame(vector<int>& piles) {
			int n = piles.size();
			
			dp.resize(n, vector<int>(n, -1));
			return recursion(0, 0, 0, n - 1, piles);
		}
		
		bool recursion(int alex, int lee, int i, int j, vector<int> &piles){
			if(i > j) 
				return (alex > lee);
			
			if(dp[i][j] != -1){
				if(dp[i][j] == 1 && alex >= lee)
					return true;
				if(dp[i][j] == 0 && alex <= lee)
					return true;
			}
			
			bool ans = false;
			ans |= recursion(alex + piles[i], lee + piles[i + 1], i + 2, j, piles);
			if(ans) return dp[i][j] = true;
			
			ans |= recursion(alex + piles[i], lee + piles[j], i + 1, j - 1, piles);
			if(ans) return dp[i][j] = true;
			
			ans |= recursion(alex + piles[j], lee + piles[j - 1], i, j - 2, piles);
			if(ans) return dp[i][j] = true;
			
			ans |= recursion(alex + piles[j], lee + piles[i], i + 1, j - 1, piles);
			return dp[i][j] = ans;
		}
	};

```

## Space and Time Complexity

* Space Complexity: O(n*n) - for dp matrix and function stack
* Time Complexity: O(4 ^ (n / 2)) - every time the recursion call we calling 4 recursion calls and the piles becomes 0 in n / 2 time