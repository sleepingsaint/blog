---
title: "Map Sum Pairs"
date: 2021-07-30T21:36:35+05:30
draft: false
tags: ["trie", "map", "unordered_map", "hashmap"]
series: "leetcode-daily"
---

# Map Sum Pairs

## Question

Implement the MapSum class:

* MapSum() 
	* Initializes the MapSum object.
* void insert(String key, int val) 
	* Inserts the key-val pair into the map. If the key already existed, the original key-value pair will be overridden to the new one.
* int sum(string prefix) 
	* Returns the sum of all the pairs' value whose key starts with the prefix.


## Constraints

Constraints:

* 1 <= key.length, prefix.length <= 50
* key and prefix consist of only lowercase English letters.
* 1 <= val <= 1000
* At most 50 calls will be made to insert and sum.


## Explanation 

In the question we can notice that, we need to retrieve all the strings which are having the given prefix. A __Trie__ data structure would be right choice to implement that. From the question it is pretty clear that we can use __map / unordered_map__ to retrieve the value of given key.

```cpp

	// Defining the Trie class
	class Trie{
	public:
		Trie* children[26];
	};

	class MapSum {
		unordered_map<string, int> um;
		Trie* trie = NULL;
	public:
		/** Initialize your data structure here. */
		MapSum() {
			um.clear();
			trie = new Trie();
		}
		
		void insert(string key, int val) {
			um[key] = val;
			Trie* root = trie;
			
			// building the Trie with given words
			for(char c: key){
				int index = c - 'a';
				if(!root->children[index])
					root->children[index] = new Trie();
				root = root->children[index];
			}
		}
		
		int sum(string prefix) {
			// getting the root with given prefix
			Trie* root = trie;
			for(char c: prefix){
				int index = c - 'a';
				// if given prefix doesn't exists return 0
				// since there exists no words with given prefix
				if(!root->children[index])
					return 0;
				root = root->children[index];
			}
			
			return getSum(root, prefix);
		}
		
		// recursively computing the sum of the values of 
		// words with given prefix
		int getSum(Trie* root, string str){
			int sum = um[str];
			for(int i = 0; i < 26; i++){
				if(root->children[i]){
					char c = 'a' + i;
					sum += getSum(root->children[i], str + c);
				}
			}
			
			return sum;
		}
	};

```

> Time Complexity: O(n), Space Complexity: O(n) 