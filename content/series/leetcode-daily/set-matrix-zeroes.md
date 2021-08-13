---
title: "Set Matrix Zeroes"
date: 2021-08-13T13:36:46+05:30
draft: false 
tags: ["array", "matrix"]
series: "leetcode-daily" 
---

## Question

Given an m x n integer matrix matrix, if an element is 0, set its entire row and column to 0's, and return the matrix.

You must do it in place.

## Examples

Input: matrix = [[1,1,1],[1,0,1],[1,1,1]]

Output: [[1,0,1],[0,0,0],[1,0,1]]
<hr />

Input: matrix = [[0,1,2,0],[3,4,5,2],[1,3,1,5]]

Output: [[0,0,0,0],[0,4,5,0],[0,3,1,0]]

## Constraints

* m == matrix.length
* n == matrix[0].length
* 1 <= m, n <= 200
* -2^31 <= matrix[i][j] <= 2^31 - 1

## Explanation

One approach for this problem is, we maintain two __hashmap__ to store which row and column should be made 0. We do this by first iterating over the matrix and if the cell is 0 then, we include the row and column number in the hashmap. In the next iteration we for every cell, we check the hashmap if the row or column of the cell is marked to be made 0 in the hashmap, if it is marked we make it 0 or else we leave it as it is.  

The space complexity of this approach is O(rows + cols), to store the rows and cols indexes.

For a more efficient space complexity solution is to use the first row and column as the hashmap's. The idea is, if any cell is 0, then we mark the column of the first row and row of the cell column as 0, indicating that in the final result cell row and column should be zero. 

Now there is a problem here. The first row and first column cell, if it is made zero we don't if we have to make the row zero or column zero or both. So to avoid this we maintain a variable to check if we have to make the column zero and the first cell is used for making the first zero.

In the filling iteration, we start filling from second row and second column, if we don't do that, let's say the first row is completely made zero so the whole matrix becomes zero which might not be the answer. To better understand this try the second example.

## Code

### Space Complexity - O(n + m)

```cpp

	class Solution {
	public:
		void setZeroes(vector<vector<int>>& matrix) {
			int m = matrix.size(), n = matrix[0].size();
			
			// hashmap to mark the rows and columns
			// to be made 0 or not
			bool row[m], col[n];
			
			// initially we mark all rows and columns as not 0
			memset(row, false, sizeof(row));
			memset(col, false, sizeof(col));
			
			// first iteration to check if we can make row or column as 0
			for(int i = 0; i < m; i++){
				for(int j = 0; j < n; j++){
					if(matrix[i][j] == 0){
						row[i] = true;
						col[j] = true;
					}
				}
			}
			
			// updating the matrix based on the hashmaps
			for(int i = 0; i < m; i++){
				for(int j = 0; j < n; j++){
					if(row[i] || col[j])
						matrix[i][j] = 0;
				}
			}
		}
	};

```

### Space Complexity - O(1)

```cpp

	class Solution {
	public:
		void setZeroes(vector<vector<int>>& matrix) {
			int m = matrix.size(), n = matrix[0].size();
			// variable to check if we need to make the first column zero
			bool isCol = false;
			
			// storing the row and column values in the first row and column
			for(int i = 0; i < m; i++){
				if(matrix[i][0] == 0)
					isCol = true;
				for(int j = 0; j < n; j++){
					if(matrix[i][j] == 0)
						matrix[i][0] = 0, matrix[0][j] = 0;
				}
			}
			
			// updating the matrix
			for(int i = 1; i < m; i++){
				for(int j = 1; j < n; j++){
					if(matrix[i][0] == 0 || matrix[0][j] == 0)
						matrix[i][j] = 0;
				}
			}
			
			// making the first row zero if first cell is zero
			if(matrix[0][0] == 0)
				for(int j = 0; j < n; j++)
					matrix[0][j] = 0;
			
			// making the first column zero if isCol variable is true
			if(isCol)
				for(int i = 0; i < m; i++)
					matrix[i][0] = 0;
		}
	};

```
## Space and Time Complexity

* Space Complexity - O(1)
* Time Complexity - O(2nm)