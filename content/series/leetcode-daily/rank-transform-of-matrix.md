---
title: "Rank Transform of Matrix"
date: 2021-08-08T16:26:38+05:30
draft: false 
tags: ["Union Find", "Disjoint Set", "matrix"]
series: "leetcode-daily" 
---

## Question

Given an m x n matrix, return a new matrix answer where answer[row][col] is the rank of matrix[row][col].

The rank is an integer that represents how large an element is compared to other elements. It is calculated using the following rules:

The rank is an integer starting from 1.

* If two elements p and q are in the same row or column, then:

	* If p < q then rank(p) < rank(q)
	* If p == q then rank(p) == rank(q)
	* If p > q then rank(p) > rank(q)

* The rank should be as small as possible.

* It is guaranteed that answer is unique under the given rules.

## Examples

Input: matrix = [[1,2],[3,4]]

Output: [[1,2],[2,3]]

Explanation:

The rank of matrix[0][0] is 1 because it is the smallest integer in its row and column. <br >
The rank of matrix[0][1] is 2 because matrix[0][1] > matrix[0][0] and matrix[0][0] is rank 1. <br >
The rank of matrix[1][0] is 2 because matrix[1][0] > matrix[0][0] and matrix[0][0] is rank 1. <br >
The rank of matrix[1][1] is 3 because matrix[1][1] > matrix[0][1], matrix[1][1] > matrix[1][0], and both matrix[0][1] and matrix[1][0] are rank 2.

<hr />

Input: matrix = [[7,7],[7,7]]

Output: [[1,1],[1,1]]

<hr />

Input: matrix = [[20,-21,14],[-19,4,19],[22,-47,24],[-19,4,19]]

Output: [[4,2,3],[1,3,4],[5,1,6],[1,3,4]]

<hr />

Input: matrix = [[7,3,6],[1,4,5],[9,8,2]]

Output: [[5,1,4],[1,2,3],[6,3,1]]

## Constraints

* m == matrix.length
* n == matrix[i].length
* 1 <= m, n <= 500
* -109 <= matrix[row][col] <= 109

## Explanation

This problem is a hard version of rank transform of a array, in which we found the rank of just one row, where as in this problem we need to find the solution for multiple rows and columns. 

Just like the array version we sort the numbers based on their value and assign the ranks accordingly. But we need to take care that same elements belong to same row and column must have the same rank (you could refer to example 3 to understand this), so we need to group all these elements and assign a common rank. For group these elements we use a __Union Find / Disjoint Set__ data structure. We group the elements based on their __row / column__ number.

## Code

```cpp

	class Solution {
	public:
		int *parent;
		
		int find(int i){
			if(parent[i] != i)
				parent[i] = find(parent[i]);
			return parent[i];
		}
		
		void merge(int i, int j){
			int par1 = find(i);
			int par2 = find(j);
			
			if(par1 == par2) 
				return;
			parent[par1] = par2;
		}
		
		vector<vector<int>> matrixRankTransform(vector<vector<int>>& matrix) {
			// store all the positions of similar elements
			map<int, vector<pair<int,int>>> val2pos;
			
			int rows = matrix.size(), cols = matrix[0].size();
			for(int i = 0; i < rows; i++){
				for(int j = 0; j < cols; j++)
					val2pos[matrix[i][j]].push_back({i, j});
			}
			
			vector<vector<int>> ans(rows, vector<int>(cols, -1));
			
			// stores the max rank for specific row and column
			int rowMaxRank[rows], colMaxRank[cols];
			memset(rowMaxRank, 0, sizeof(rowMaxRank));
			memset(colMaxRank, 0, sizeof(colMaxRank));
			
			// iterating over the elements in sorted order
			// since we are using map, the keys are already sorted
			for(auto it = val2pos.begin(); it != val2pos.end(); it++){
				// initialing the union find / disjoint set for this element
				parent = new int[rows + cols];
				for(int i = 0; i < rows + cols; i++)
					parent[i] = i;
				
				// merging the all the elements with same row / column
				for(pair<int, int> p: it->second){
					merge(p.first, p.second + rows);
				}
				
				// find the common rank
				// the common rank will be the one which is largest
				map<int, int> root2rank;
				for(pair<int, int> p: it->second){
					int i = p.first, j = p.second;
					int root = find(p.first);
					root2rank[root] = max(root2rank[root], max(rowMaxRank[i], colMaxRank[j]) + 1);
				}
				
				// updating the ans and rowMaxRank, colMaxRank array
				for(pair<int, int> p: it->second){
					int i = p.first, j = p.second;
					int root = find(i);
					int rank = root2rank[root];
					ans[i][j] = rank;
					rowMaxRank[i] = rank;
					colMaxRank[j] = rank;
				}
			}
			
			return ans;
		}
	};

```

## Space and Time Complexity

* Time Complexiy: O(rows x cols x logN) - logN for inserting elements into the map
* Space Complexity: O(rows x cols)