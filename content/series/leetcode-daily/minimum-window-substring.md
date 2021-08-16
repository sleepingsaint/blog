---
title: "Minimum Window Substring"
date: 2021-08-16T13:23:57+05:30
draft: false 
tags: ["string", "sliding window", "hashmap"]
series: "leetcode-daily" 
---

## Question

Given two strings s and t of lengths m and n respectively, return the minimum window substring of s such that every character in t (including duplicates) is included in the window. If there is no such substring, return the empty string "".

The testcases will be generated such that the answer is unique.

A substring is a contiguous sequence of characters within the string.

## Examples

Input: s = "ADOBECODEBANC", t = "ABC"

Output: "BANC"

Explanation: The minimum window substring "BANC" includes 'A', 'B', and 'C' from string t.
<hr />

Input: s = "a", t = "a"

Output: "a"

Explanation: The entire string s is the minimum window.
<hr />

Input: s = "a", t = "aa"

Output: ""

Explanation: Both 'a's from t must be included in the window. Since the largest window of s only has one 'a', return empty string.

## Constraints

* m == s.length
* n == t.length
* 1 <= m, n <= 105
* s and t consist of uppercase and lowercase English letters.

## Explanation

A brute approach would be to get all the substrings and check if it contains all the characters in t and pick the minimum length substring. A much better approach is to use the __sliding window__ technique. As the name indicates we create a window and check if the requirements are met or not, if not met we move the window, if met we try to contract the window. The below shows how it looks while implementing.

<div style="display: flex; justify-content: center">

![Sliding Window Image](https://www.researchgate.net/profile/Ali-Raza-76/publication/325158306/figure/fig5/AS:681968115134465@1539605277480/Sliding-window-technique.ppm)
</div>

In this problem our requirement is to have all the characters of the string t.

We start with two pointers initially at the start of the string s, then we move the end pointer untill we met the requirement and when we met the requirement we minimise the window. To check if we met the requirement, we maintain a variable to track the count of the characters in the current window and an __hashmap__ to check if the current character is in the string t and another __hashmap__ to keep track of all characters in the current window.

The variable we maintaing is counting only the characters which are both in the current window and in the string t, where as the window hashmap keep track of all the characters even though they don't exist in the string t. 

## Code

```cpp

	class Solution {
	public:
		string minWindow(string s, string t) {
			// if one of the strings is empty then the answer is also empty string
			if(s.length() == 0 && t.length() == 0)
				return "";
			
			// hashmap to store the characters of string t
			unordered_map<char, int> tcount;
			for(char c: t)
				tcount[c]++;
			
			// variables to return the answer / substring
			int minLength = -1, start = 0;
			
			// start and end pointers of the window
			int i = 0, j = 0;
			
			// hashmap to keep track the characters in window
			unordered_map<char, int> wcount;

			// this keep track of the characters visited
			int count = 0;
			
			while(j < s.length()){
				// increase the count of the character in the current window
				wcount[s[j]]++;
				
				// if we got all occurences of a character in the window we increment the count
				if(tcount.find(s[j]) != tcount.end() && tcount[s[j]] == wcount[s[j]])
					count++;
				
				// if we found all the characters in the current window
				// we try to minimize the window
				while(count == tcount.size() && i <= j){
					
					// update the answer
					if(minLength == -1 || minLength > j - i + 1){
						minLength = j - i + 1;
						start = i;
					}
					
					wcount[s[i]]--;
					
					// if we are short of some characters we decrement the count variable 
					// indicating that the window is not valid anymore
					if(tcount.find(s[i]) != tcount.end() && wcount[s[i]] < tcount[s[i]])
						count--;
					
					i++;
				}
				
				j++;
			}

			// if we found the answer we return the substring else empty string
			return minLength == -1 ? "" : s.substr(start, minLength);
		}
	};

```

## Space and Time Complexity
N - length of string t, M - length of string s

* Space Complexity: O(N + M) - for storing the characters in the hashmap
* Time Complexity: O(2xM) - atmost we encounter each variable twice so the time complexity is 2xM