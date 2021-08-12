---
title: "Group Anagrams"
date: 2021-08-12T14:09:06+05:30
draft: false 
tags: ["hashmap", "string", "sorting"]
series: "leetcode-daily" 
---

## Question

Given an array of strings strs, group the anagrams together. You can return the answer in any order.

An Anagram is a word or phrase formed by rearranging the letters of a different word or phrase, typically using all the original letters exactly once.

## Examples

Input: strs = ["eat","tea","tan","ate","nat","bat"]

Output: [["bat"],["nat","tan"],["ate","eat","tea"]]
<hr />

Input: strs = [""]

Output: [[""]]
<hr />

Input: strs = ["a"]

Output: [["a"]]
<hr />

## Constraints

* 1 <= strs.length <= 104
* 0 <= strs[i].length <= 100
* strs[i] consists of lower-case English letters.

## Explanation

As mentioned in the question we need to group all the strings which are anagrams. But how to know if two strings are anagrams, we can use nested for loop to check how many other anagram strings exists. But if we think a bit more we can see that by sorting all the strings which are anagrams looks same.

Example: 
* Before sorting ["eat", "tea", "ate"] 
* After sorting ["aet", "aet", "aet"]

So, for every given string we sort the string (in more compact way, we sort the characters of the string) and store the string with key as sorted string and value as original string. For storing we can __hashmap__. But there might be more than one string with same anagram, so as in the value field of hashmap we store array of strings, instead of single string, to store all the anagram strings.

## Code

```cpp

	class Solution {
	public:
		vector<vector<string>> groupAnagrams(vector<string>& strs) {
			// hashmap to store all the anagrams
			map<string, vector<string>> m;

			for(string str: strs){
				
				// sorting the string to 
				// get the key of all the related anagram
				string tmp = str;
				sort(tmp.begin(), tmp.end());

				// adding the string to the corresponding anagrams group
				m[tmp].push_back(str);
			}

			// storing all the anagram groups in the answer
			vector<vector<string>> vec;
			for(auto it = m.begin(); it != m.end(); it++)
				vec.push_back(it->second);
			return vec;
		}
	};

```

## Space and Time Complexity

* Space Complexity: O(N) - to store the N strings in the hashmap
* Time Complexity: O(N*nlog(n)) - For every string we are sorting string. sorting takes nlogn time where n is max length of the given strings 