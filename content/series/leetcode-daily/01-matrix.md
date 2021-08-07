---
title: "01 Matrix"
date: 2021-07-31T13:00:13+05:30
draft: false 
series: "leetcode-daily"
tags: ["bfs", "matrix"]
---

## Question

Given an m x n binary matrix mat, return the distance of the nearest 0 for each cell.

The distance between two adjacent cells is 1.

## Examples

Input: mat = [[0,0,0],[0,1,0],[0,0,0]]

Output: [[0,0,0],[0,1,0],[0,0,0]]

<hr />

Input: mat = [[0,0,0],[0,1,0],[1,1,1]]

Output: [[0,0,0],[0,1,0],[1,2,1]]

## Constraints

* m == mat.length
* n == mat[i].length
* 1 <= m, n <= 104
* 1 <= m * n <= 104
* mat[i][j] is either 0 or 1.
* There is at least one 0 in mat.

## Explanation

Since the matrix is a 2 Dimensional space we can start searching from individual __1's__ till we find a __0__. The distance along this path might be shortest distance or might not be. If it is we update the answer if not we break the search along the current path and do it another path. 

I started the __bfs (Breadth First Search)__ search from __0's__ instead of __1__ and updated the distances of __1's__ if I encountered any. So we use __queue__ data structure for the bfs. Once we are at an element be it either 1 or 0 we update all the neighbors and if any neighbor gets updated we push that neighbor into the queue and continue to update the path along that neighbor.

## Code

```cpp

	class Solution {
	public:
		vector<vector<int>> updateMatrix(vector<vector<int>>& mat) {
			int m = mat.size();
			int n = mat[0].size();
			
			vector<vector<int>> ans(m, vector<int>(n, INT_MAX));
			
			// initializing the queue 
			queue<pair<int, int>> q;
			for(int i = 0; i < m; i++){
				for(int j = 0; j < n; j++){
					if(mat[i][j] == 0){
						ans[i][j] = 0;
						q.push({i, j});
					}
				}
			}
			
			// this 2 arrays can be used to 
			// loop through the adjacent cells
			int x[] = {-1, 0, 0, 1};
			int y[] = {0, 1, -1, 0};
			
			// running the bfs untill all the answers are updated
			while(!q.empty()){
				pair<int, int> front = q.front();
				q.pop();
				
				int i = front.first, j = front.second;
				
				// checking for all the adjacent neigbhours
				for(int k = 0; k < 4; k++){
					int nei_i = i + x[k], nei_j = j + y[k];
					
					// checking if the indexes are not out of limits 
					// and if the ans of the neigbhor is greater than the current ans
					// if the ans is already less we don't need to update the ans of the neighbor
					if(nei_i < 0 || nei_i >= m || nei_j < 0 || nei_j >= n || ans[nei_i][nei_j] < ans[i][j] + 1)
						continue;
					ans[nei_i][nei_j] = ans[i][j] + 1;
					q.push({nei_i, nei_j});
				}
			}
			
			return ans;
		}
	};

```

## Space and Time Complexity

* Time Complexity: O(nm) - for iterating over all the nodes
* Space Complexity: O(2nm)) - for storing the ans and at worst condition the whole matix is of 0's for queue stores all the nm nodes again hence 2(nm);