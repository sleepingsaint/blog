---
title: "Valid Sudoku"
date: 2021-08-20T13:39:26+05:30
draft: false 
tags: ["hashset", "matrix"]
series: "leetcode-daily" 
---

## Question

Determine if a 9 x 9 Sudoku board is valid. Only the filled cells need to be validated according to the following rules:

Each row must contain the digits 1-9 without repetition.
Each column must contain the digits 1-9 without repetition.
Each of the nine 3 x 3 sub-boxes of the grid must contain the digits 1-9 without repetition.

Note:

A Sudoku board (partially filled) could be valid but is not necessarily solvable.
Only the filled cells need to be validated according to the mentioned rules.

## Examples

<div>

<img style="margin: 0 auto;" src="./sudoku.png">

Input: board = 

[["5","3",".",".","7",".",".",".","."] <br /> 
,["6",".",".","1","9","5",".",".","."]<br />
,[".","9","8",".",".",".",".","6","."]<br />
,["8",".",".",".","6",".",".",".","3"]<br />
,["4",".",".","8",".","3",".",".","1"]<br />
,["7",".",".",".","2",".",".",".","6"]<br />
,[".","6",".",".",".",".","2","8","."]<br />
,[".",".",".","4","1","9",".",".","5"]<br />
,[".",".",".",".","8",".",".","7","9"]]<br />

Output: true
<hr />

Input: board = 

[["8","3",".",".","7",".",".",".","."]<br />
,["6",".",".","1","9","5",".",".","."]<br />
,[".","9","8",".",".",".",".","6","."]<br />
,["8",".",".",".","6",".",".",".","3"]<br />
,["4",".",".","8",".","3",".",".","1"]<br />
,["7",".",".",".","2",".",".",".","6"]<br />
,[".","6",".",".",".",".","2","8","."]<br />
,[".",".",".","4","1","9",".",".","5"]<br />
,[".",".",".",".","8",".",".","7","9"]]<br />

Output: false<br />

Explanation: Same as Example 1, except with the 5 in the top left corner being modified to 8. Since there are two 8's in the top left 3x3 sub-box, it is invalid.

</div>

## Constraints

* board.length == 9
* board[i].length == 9
* board[i][j] is a digit or '.'

## Explanation

We can check the validity of the position in the row and in the col by just iterating in the same row by keeping the column index constant and in the same column keeping the row index constant, but the tricky problem is to check if the digit is valid in the current box. We have three boxes in a row and three boxes in a column, so we could use a set to store the digits in the box.

## Code

```cpp

	class Solution {
	public:
		bool isValidSudoku(vector<vector<char>>& board) {
			// set array to store the elements in the current box
			set<char> box[3][3];
			
			for(int i = 0; i < 9; i++){
				for(int j = 0; j < 9; j++){
					if(board[i][j] == '.')
						continue;

					char c = board[i][j];
					
					int x = i + 1, y = j + 1;

					// checking if digit is valid in the row
					for(; x < 9; x++)
						if(board[x][j] == c)
							return false;
					for(x = 0; x < i; x++)
						if(board[x][j] == c)
							return false;
					
					// checking if digit is valid in the column
					for(; y < 9; y++)
						if(board[i][y] == c)
							return false;
					for(y = 0; y < j; y++)
						if(board[i][y] == c)
							return false;
					
					// checking if the digit is valid in the box
					// all the elements in the box have the same index
					// if we divide their indexes with 3
					x = i / 3, y = j / 3;
					if(box[x][y].find(c) != box[x][y].end())
						return false;
					
					// else add the digit to the current box
					// to check for other digits
					box[x][y].insert(c);
				}
			}
			// if all the digits are in valid positions
			// we return true 
			return true;
		}
	};

```

## Space and Time Complexity

* Space Complexity: O(81) - in the worst case we store all the 81 elements
* Time Complexity: O(81 * 81 * (18 + log9)) - 18 for checking the row and column digits and log9 for storing the digit in the boxes.