---
title: "Longest Uncommon Subsequence"
date: 2021-08-29T01:45:42+05:30
draft: false 
tags: ["string", "array", "sorting", "hashmap"]
series: "leetcode-daily" 
---

## Question

Given an array of strings strs, return the length of the longest uncommon subsequence between them. If the longest uncommon subsequence does not exist, return -1.

An uncommon subsequence between an array of strings is a string that is a subsequence of one string but not the others.

A subsequence of a string s is a string that can be obtained after deleting any number of characters from s.

For example, "abc" is a subsequence of "aebdc" because you can delete the underlined characters in "aebdc" to get "abc". Other subsequences of "aebdc" include "aebdc", "aeb", and "" (empty string).

## Examples

Input: strs = ["aba","cdc","eae"] <br />
Output: 3
<hr />

Input: strs = ["aaa","aaa","aa"] <br />
Output: -1

## Constraints

* 1 <= strs.length <= 50
* 1 <= strs[i].length <= 10
* strs[i] consists of lowercase English letters.

## Explanation

If you look closely we can see that the longest string which doesnot contain duplicate in the given strings. We also need to take care of another case, which is the string we selected should not be a subsequence of any other strings. 

## Code

```cpp

	class Solution {
	public:
		// helper function to check if s2 is a subsequence of s1
		bool isSubSeq(string &s1, string &s2){
			int n1 = s1.length(), n2 = s2.length();
			int i = 0, j = 0, count = 0;
			
			while (i < n1 && j < n2){
				if(s1[i] == s2[j]){
					count++;
					j++;
				}
				i++;
			}
			
			if(count == n2) return true;
			return false;
		}
		
		int findLUSlength(vector<string>& strs) {
			// sorting the strings in increasing order of length
			// so that once we find a valid string we don't need to iterate 
			// over other strings, since the current string would be the longest
			sort(strs.begin(), strs.end(), [](string &s1, string &s2){
				return s1.length() > s2.length();
			});
			
			// hashmap to count the occurences of the strings
			unordered_map<string, int> count;
			for(string &s: strs)
				count[s]++;
			
			for(int i = 0; i < strs.size(); i++){
				// we only check if there are no duplicates
				// if duplicates exists the value would be more than 1
				if(count[strs[i]] == 1){
					// checking if the current string is a 
					// subsequence to any other strings
					bool isSeq = false;
					for(int j = 0; j < i && !isSeq; j++){
						isSeq |= isSubSeq(strs[j], strs[i]);
					}
					
					// if it is not a subsequence return the string length
					if(!isSeq)
						return strs[i].length();
				}
			}
			
			// if we didn't find any answer return -1
			return -1;
		}
	};

```

## Space and Time Complexity
N - number of strings, M - max length of the given strings

* Space Complexity: O(N) - in worst case we would be given all distinct strings, so we store all the strings in the hashmap
* Time Complexity: O(NlogN + N + NxNxM) - NlogN for sorting the strings, N for filling the hashmap, NM - for every string we are checking if it is subsequence of any other strings which are of greater length than the current string. There will be at max N strings, and M for checking if it is a subsequence or not. 