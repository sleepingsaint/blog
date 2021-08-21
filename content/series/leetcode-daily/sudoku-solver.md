---
title: "Sudoku Solver"
date: 2021-08-21T13:16:23+05:30
draft: false
tags: ["dfs", "recursion", "back tracking"]
series: "leetcode-daily"
---

## Question

Write a program to solve a Sudoku puzzle by filling the empty cells.

A sudoku solution must satisfy all of the following rules:

1. Each of the digits 1-9 must occur exactly once in each row.
2. Each of the digits 1-9 must occur exactly once in each column.
3. Each of the digits 1-9 must occur exactly once in each of the 9 3x3 sub-boxes of the grid.

The '.' character indicates empty cells.

## Examples

<div style="display: flex; justify-content: space-around">
	<img src="unsolved_board.png" />
	<img src="solved_board.png" />
</div>

Input: board =

[
["5","3",".",".","7",".",".",".","."],<br />
["6",".",".","1","9","5",".",".","."],<br />
[".","9","8",".",".",".",".","6","."],<br />
["8",".",".",".","6",".",".",".","3"],<br />
["4",".",".","8",".","3",".",".","1"],<br />
["7",".",".",".","2",".",".",".","6"],<br />
[".","6",".",".",".",".","2","8","."],<br />
[".",".",".","4","1","9",".",".","5"],<br />
[".",".",".",".","8",".",".","7","9"]
]

Output:

[
["5","3","4","6","7","8","9","1","2"],<br />
["6","7","2","1","9","5","3","4","8"],<br />
["1","9","8","3","4","2","5","6","7"],<br />
["8","5","9","7","6","1","4","2","3"],<br />
["4","2","6","8","5","3","7","9","1"],<br />
["7","1","3","9","2","4","8","5","6"],<br />
["9","6","1","5","3","7","2","8","4"],<br />
["2","8","7","4","1","9","6","3","5"],<br />
["3","4","5","2","8","6","1","7","9"]
]

## Constraints

- board.length == 9
- board[i].length == 9
- board[i][j] is a digit or '.'.
- It is guaranteed that the input board has only one solution.

## Explanation

This problem is next step of the yesterday's problem [valid sudoku](/blog/series/leetcode-daily/valid-sudoku/). To this solve this problem, we use __backtracking and dfs__ technique to try to fill the empty cells and validate them and if it is valid we keep forward else we backtrack and try with another combination.

## Code

```cpp

	class Solution {
	public:
		// set array to store the elements in the current box
		set<char> box[3][3];
		vector<vector<char>> res;

		// checks if the character c can be placed in the given position (i, j)
		bool isValid(vector<vector<char>> &board, int i, int j, char c){
			// checking if the digit is valid in the box
			// all the elements in the box have the same index
			// if we divide their indexes with 3
			if(box[i / 3][j / 3].find(c) != box[i / 3][j / 3].end())
				return false;

			// checking if digit is valid in the row
			for(int x = 0; x < i; x++)
				if(board[x][j] == c)
					return false;
			for(int x = i + 1; x < 9; x++)
				if(board[x][j] == c)
					return false;

			// checking if digit is valid in the column
			for(int y = 0; y < 9; y++)
				if(board[i][y] == c)
					return false;
			for(int y = j + 1; y < 9; y++)
				if(board[i][y] == c)
					return false;

			return true;
		}

		void solveSudoku(vector<vector<char>>& board) {
			// initially storing all the given cells
			for(int i = 0; i < 9; i++){
				for(int j = 0; j < 9; j++){
					if(board[i][j] == '.'){
						continue;
					}
					box[i / 3][j / 3].insert(board[i][j]);
				}
			}

			// recursively solving the board using dfs approach
			solve(board, 0, 0);
			board = res;
		}

		void solve(vector<vector<char>> &board, int i, int j){
			// if we reached the 9th row that
			// means the board is filled completely and is valid sudoku
			// so we store the answer and exit the recursion
			if(i == 9){
				res = board;
				return;
			}

			// finding the empty cell in the current row
			for(; j < 9; j++){
				if(board[i][j] == '.')
					break;
			}

			// if there is no empty cell in the current row
			// then move to the next row of the board
			if(j == 9)
				solve(board, i + 1, 0);
			else{
				// we try to fill the board with characters one by one
				// and check if the character is valid or not
				// else we backtrack
				for(char c = '1'; c <= '9'; c++){

					// checking if this character c can be placed in this position
					if(isValid(board, i, j, c)){
						// if we can, then place the character and 
						// recursively solve the board
						board[i][j] = c;
						box[i / 3][j / 3].insert(c);

						// calling the recursive solve function
						solve(board, i, j);

						// backtracking step
						// we replace the character with the empty cell
						board[i][j] = '.';
						box[i / 3][j / 3].erase(c);
					}
				}
			}
		}
	};

```

## Space and Time Complexity
* Space Complexity: O(81 + 81) - we need to store all the number in boxes matrix, another 81 is for the recursion stack
* Time Complexity: O(81^9) - at every empty cell we can choose 9 different options, in total we have 81 cells in the worst case we have 81 empty cells so the time complexity in that case would be 81 power 9.
