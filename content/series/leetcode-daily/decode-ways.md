---
title: "Decode Ways"
date: 2021-08-18T16:33:01+05:30
draft: false 
tags: ["string", "recursion", "dynamic programming"]
series: "leetcode-daily" 
---

## Question

A message containing letters from A-Z can be encoded into numbers using the following mapping:

'A' -> "1"
'B' -> "2"
...
'Z' -> "26"

To decode an encoded message, all the digits must be grouped then mapped back into letters using the reverse of the mapping above (there may be multiple ways). For example, "11106" can be mapped into:

* "AAJF" with the grouping (1 1 10 6)
* "KJF" with the grouping (11 10 6)

Note that the grouping (1 11 06) is invalid because "06" cannot be mapped into 'F' since "6" is different from "06".

Given a string s containing only digits, return the number of ways to decode it.

The answer is guaranteed to fit in a 32-bit integer.

## Examples

Input: s = "12" <br />
Output: 2 <br />
Explanation: "12" could be decoded as "AB" (1 2) or "L" (12).
<hr/>

Input: s = "226" <br />
Output: 3 <br />
Explanation: "226" could be decoded as "BZ" (2 26), "VF" (22 6), or "BBF" (2 2 6).
<hr />

Input: s = "0" <br />
Output: 0 <br />
Explanation: There is no character that is mapped to a number starting with 0.
The only valid mappings with 0 are 'J' -> "10" and 'T' -> "20", neither of which start with 0.
Hence, there are no valid ways to decode this since all digits need to be mapped.
<hr />

Input: s = "06" <br />
Output: 0 <br />
Explanation: "06" cannot be mapped to "F" because of the leading zero ("6" is different from "06").

## Constraints

* 1 <= s.length <= 100
* s contains only digits and may contain leading zero(s).

## Explanation

For this problem we the first idea we get is to use __recursion__. We map first or first two characters to some characters and try to map the remaining string to something else. This will work fine for smaller input's but given the constraints we still might encounter TLE. 

So another approach would be solve the problem from the bottom instead of the top using __dynamic programming__. Before we mapped first characters and checked the number of strings in the remaining strings, now we first map the remaining string and then try to map the first characters, this way the code runs in O(N). 

## Code

### Recursion

```cpp

	class Solution {
	public:
		int numDecodings(string s) {
			// variable to store all the unique strings
			set<string> ways;
			
			// recursively calling to make mappings 
			dfs(s, ways, 0, "");
			return ways.size();
		}
		
		void dfs(string &s, set<string> &ways, int i, string curr){
			int n = s.length();
			if(i == n) {
				// if we reached the end we add the current 
				// mapped string to the set
				ways.insert(curr);
				return;
			}
			
			// if the current starting character is
			// 0 we cannot map as mentioned in question
			if(s[i] == '0')
				return;

			// we map the first character and then remaining string
			int num = stoi(s.substr(i, 1)) - 1;
			dfs(s, ways, i + 1, curr + char(65 + num));
			
			// we map the first 2 characters and then remaining string
			num = stoi(s.substr(i, 2)) - 1;
			if(i != n - 1 && num < 26){
				dfs(s, ways, i + 2, curr + char(65 + num));
			}
		}
	};

```

### dynamic programming

```cpp

	class Solution {
	public:
		int numDecodings(string s) {
			int n = s.length();

			int dp[n];
			
			dp[n - 1] = s[n - 1] == '0' ? 0 : 1;
			
			for(int i = n - 2; i >= 0; i--){
				// this loops calculates the number of 
				// decodings if string[i:n-1] th indexes are given

				dp[i] = 0;
				
				int num1 = stoi(s.substr(i, 1));
				// if current number is 0 we skip
				if(num1 == 0)
					continue;
				
				// mapping single character
				// this statement also checks if the index is n - 1
				// then the answer for the dp[i] will be 1
				dp[i] += i + 1 < n ? dp[i + 1] : 1;
				
				// mapping 2 characters
				int num2 = stoi(s.substr(i, 2));
				if(num2 <= 26)
					// this statement also checks if the index is n - 2
					// then the answer for the dp[i] will be 2 since we can map
					dp[i] += i + 2 < n ? dp[i + 2] : 1;
			}

			return dp[0];
		}
	};

```

## Space and Time Complexity

N - length of the given string

* Space Complexity: O(N) - for storing the dp table
* Time Complexity: O(N) - we are iterating every character only once
