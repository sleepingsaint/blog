---
title: "Palindrome Partitioning 2"
date: 2021-08-07T18:57:45+05:30
draft: false 
tags: ["string", "dynamic programming"]
series: "leetcode-daily" 
---

## Question

Given a string s, partition s such that every substring of the partition is a palindrome.

Return the minimum cuts needed for a palindrome partitioning of s.

## Examples

Input: s = "aab"

Output: 1

Explanation: The palindrome partitioning ["aa","b"] could be produced using 1 cut.

<hr />

Input: s = "a"

Output: 0
<hr />
Input: s = "ab"

Output: 1

## Constraints

* 1 <= s.length <= 2000
* s consists of lower-case English letters only.

## Explanation

The approach for this problem is very much similar to __parlidrome partitioning__ problem. Here we can use recursion to get all the palindromic substrings arrays and pick out the smallest array in the answer, which would give us the minimum cuts required to split the given string to palindromes.

But using the recursion, in this problem would give us a TLE, so we use another array using to calculate the minimum cuts using __dynamic programming__. First we construct a dp table to check if a given substring is palindrome or not, then we use cuts array to find the minimum cuts. 

In cuts array we iterate over elements and check if the current substring from the 0th index is palindrome or not. If it is, then the cuts required is 0, else we iterate in the range of [0, curr index). At the iterating index let's say ith index, we check if substring of index range (i + 1, curr index) is palindrome or not, if it is palindrome we try to update the answer of cuts[curr index] = min(cuts[curr index], 1 + cuts[i]). 

Here 1 indicates the one cut and the cuts[i] indicates the number of minimum cuts required to make the substring (0, i) a palindrome.


## Code

```cpp

	class Solution {
	public:
		int minCut(string s) {
			int n = s.length();
			
			// construncting is palindrome dp table
			bool isPalindrome[n][n];
			memset(isPalindrome, false, sizeof(isPalindrome));
			
			for(int i = 0; i < n; i++) isPalindrome[i][i] = true;
			for(int i = 0; i < n - 1; i++) isPalindrome[i][i + 1] = s[i] == s[i + 1];
			for(int l = 3; l <= n; l++){
				for(int i = 0; i < n - l + 1; i++){
					int j = i + l - 1;
					isPalindrome[i][j] = s[i] == s[j] && isPalindrome[i + 1][j - 1];
				}
			}
			
			// constructing the cuts array
			int cuts[n];
			for(int i = 0; i < n; i++){
				if(isPalindrome[0][i])
					cuts[i] = 0;
				else{
					// iterating over the range of [0, curr index)
					int temp = i + 1;
					for(int k = 0; k < i; k++){
						if(isPalindrome[k + 1][i] && temp > cuts[k] + 1){
							temp = cuts[k] + 1;
						}
					}
					cuts[i] = temp;
				}
			}
			
			return cuts[n - 1];
		}
	};

```

## Space and Time Complexity
* Space Complexity: O(N^2 + N) - for dp table and cuts table 
* Time Complexity: O(N^2) - for dp table construction and also cuts table construction
