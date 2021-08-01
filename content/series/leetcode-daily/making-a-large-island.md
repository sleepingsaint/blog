---
title: "Making a Large Island"
date: 2021-08-01T15:39:51+05:30
draft: false
tags: ["Union Find", "Disjoint Set"]
series: "leetcode-daily" 
---

## Question

You are given an n x n binary matrix grid. You are allowed to change at most one 0 to be 1.
Return the size of the largest island in grid after applying this operation.

An island is a 4-directionally connected group of 1s.

## Examples

Input: grid = [[1,0],[0,1]]
Output: 3
Explanation: Change one 0 to 1 and connect two 1s, then we get an island with area = 3.

Input: grid = [[1,1],[1,0]]
Output: 4
Explanation: Change the 0 to 1 and make the island bigger, only one island with area = 4.

Input: grid = [[1,1],[1,1]]
Output: 4
Explanation: Can't change any 0 to 1, only one island with area = 4.

## Constraints

* n == grid.length
* n == grid[i].length
* 1 <= n <= 500
* grid[i][j] is either 0 or 1.

## Explanation

A naive approach would be to run a helper function to calculate the largest island for every all __0's__. This would give us __TLE (Time Limit Error)__ because the time complexity would be __O(n^4)__. A better approach would be to form all the islands with 1's and then for every 0 we encounter, we check if making this 0 as 1 would give us the more area / joins two islands together. Since we can multiple islands and we need to group them together the __Disjoint Set / Union Find__ data structure would be a right fit for this problem. While performing the merging / union operation in Disjoint Set we also need to keep track of area of the current island.

## Code

```cpp

	// defining the union find class
	class UnionFind{
		private:
			// in this class the rank and path compression 
			// is used to reduce the time complexity
			int *parent, *rank, *area;

		public:
			// initializing the variables
			UnionFind(int n){
				parent = new int[n];
				rank = new int[n];
				area = new int[n];
				for(int i = 0; i < n; i++){
					parent[i] = i;
					rank[i] = 0;
					area[i] = 1;
				}
			}
		
			// get the parent of the given element
			int find(int n){
				if(parent[n] != n)
					parent[n] = find(parent[n]);
				return parent[n];
			}
			
			int getArea(int n) {
				return area[n];
			}
		
			// merging two elements together in a group
			void merge(int n, int m){
				int par1 = find(n);
				int par2 = find(m);
				
				if(par1 == par2) return;
				
				// updating the parents, rank and area
				if(rank[par1] > rank[par2]) {
					parent[par2] = par1;
					area[par1] += area[par2];
				}else if(rank[par2] > rank[par1]) {
					parent[par1] = par2;
					area[par2] += area[par1];
				}else {
					parent[par2] = par1;
					rank[par1]++;
					area[par1] += area[par2];
				}
				
			}
	};

	class Solution {
	public:
		int largestIsland(vector<vector<int>>& grid) {
			int rows = grid.size();
			int cols = grid[0].size();
			
			UnionFind uf = UnionFind(rows * cols);

			int dir_x[] = {-1, 0, 0, 1};
			int dir_y[] = {0, -1, 1, 0};
			
			// forming the islands
			for(int i = 0; i < rows; i++){
				for(int j = 0; j < cols; j++){
					if(grid[i][j] == 1){
						for(int k = 0; k < 4; k++){
							int nei_i = i + dir_x[k], nei_j = j + dir_y[k];
							// checking if the neigbhor indexes are still inside the grid
							// and the neighbor is not 0
							if(nei_i < 0 || nei_i >= rows || nei_j < 0 || nei_j >= cols || grid[nei_i][nei_j] == 0)
								continue;
							// adding the neigbhors to the island
							uf.merge(i*rows + j, nei_i*rows + nei_j);
						}
					}
				}
			}
			
			// to check if the grid contains any zero
			// if no zero then ans would be rows*cols
			bool hasZero = false;
			int ans = 1;

			// checking if there exists any 0 which can be changed to 1
			// to give more area
			for(int i = 0; i < rows; i++){
				for(int j = 0; j < cols; j++){
					if(grid[i][j] == 0){
						hasZero = true;

						// this set is check if the islands on the 
						// four directions are different or same.
						set<int> roots;
						int temp_area = 1;
						for(int k = 0; k < 4; k++){
							int nei_i = i + dir_x[k], nei_j = j + dir_y[k];
							
							// checking if the neigbhor indexes are still inside the grid
							// and the neighbor is not 0
							if(nei_i < 0 || nei_i >= rows || nei_j < 0 || nei_j >= cols || grid[nei_i][nei_j] == 0)
								continue;
							int root = uf.find(nei_i*rows + nei_j);

							// checking if the island is not yet added
							if(roots.find(root) == roots.end()){
								roots.insert(root);
								temp_area += uf.getArea(root);
							}
						}
						
						// updating the answer after every loop
						ans = max(ans, temp_area);
					}
				}
			}
			if(!hasZero) return rows*cols;
			return ans;
		}
	}
```
## Space and Time Complexity

Space Complexity: O(n^2)
Time Complexity: O(n^2)

n - max(rows, cols)