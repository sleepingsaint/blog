---
title: "Remove Boxes"
date: 2021-08-14T22:30:00+05:30
draft: false 
tags: ["dynammic programming", "memoization", "array"]
series: "leetcode-daily" 
---

## Question

You are given several boxes with different colors represented by different positive numbers.

You may experience several rounds to remove boxes until there is no box left. Each time you can choose some continuous boxes with the same color (i.e., composed of k boxes, k >= 1), remove them and get k * k points.

Return the maximum points you can get.

## Examples

Input: boxes = [1,3,2,2,2,3,4,3,1]

Output: 23

Explanation:

[1, 3, 2, 2, 2, 3, 4, 3, 1] <br/>
----> [1, 3, 3, 4, 3, 1] (3x3=9 points)<br/> 
----> [1, 3, 3, 3, 1] (1x1=1 points)<br/> 
----> [1, 1] (3x3=9 points)<br/> 
----> [ ] (2x2=4 points)

<hr />

Input: boxes = [1,1,1]

Output: 9

<hr />

Input: boxes = [1]

Output: 1

## Constraints

* 1 <= boxes.length <= 100
* 1 <= boxes[i] <= 100

## Explanation

I must confess that this is one of the hard problems which is difficult to come up with a solution (atleast for me) unless you did lot of hard work. 

As explained in the question, we take the continuous array of same color and add the 2 power of the length of the array and add it to the answer, now the question is, which continuous array should be picked. Here if we pick the array with largest length we might not get the maximum answer, in simple terms __greedy__ approach doesn't work, for example in example 1, from step 1 to step2 we removed [4] not [3, 3, 3] to get the maximum answer. 

We might try with the recursion approach where we remove some continous array and continue the recursion and when we finally reach a state where the array is empty, we update the answer if answer is less than the current recursion answer. The problem with this approach is the number of states is very large, because we are generating (length of array) ^ (length of array).

N - length of array

* 1st recursion call - N recursion calls
* 2nd recursion call - N - 1 recursion calls

and so on. So total states would be (N x N - 1 x N - 2 x N - 3 x .... x 1). There might be some duplicate states but even after handling the duplicate states by using __memoization__, the number of states would be large.

The time complexity would be __total states x time to compute each state__. 

So now we need to try to decrease the number of states. We use __dynamic programming__, we split the bigger problem into smaller sub problems. So the bigger problem is solve in the range of [0, boxes.length - 1] range. For some sub problem say in the range [i, j], to solve this we need to know if i is part of any continuous array, for that we use another variable k, which indicates the number of same colored boxes to the left of index i. 

Now if the i is part of any cont. array we can take it into account or we don't take it into account, since it is not required to take the entire continuous array. If it is not part of any array, we just take that single element and solve the remaining range [i + 1, j] normally. Now, there is another case we can try, in the range of [i, j], we iterate and if any element is of same color as the box at ith index we try removing the in between boxes and check if it can improve the answer or not.  

The above explanation can be better understood from going through the code.

## Code

```cpp

	class Solution {
	public:
		// this dp matrix is used for memoization
		// it eleminates the processing of same states again
		int dp[100][100][100];
		
		int removeBoxes(vector<int>& boxes) {
			memset(dp, -1, sizeof(dp));
			return util(boxes, 0, boxes.size() - 1, 0);
		}
		
		int util(vector<int> &boxes, int i, int j, int k){
			if(i > j) return 0;
			if(dp[i][j][k] != -1) return dp[i][j][k];
			
			// initially finding the answer by taking 
			// current box and previous k same colored boxes 
			// and solving the remaining range
			int res = (k + 1)*(k + 1) + util(boxes, i + 1, j, 0);
			

			for(int it = i + 1; it <= j; it++){
				// if we find the same colored box as the box at ith index
				// we remove the boxes in between the boxes ie in range [i + 1, it - 1] 
				// and solve the new range
				if(boxes[it] == boxes[i]){
					int count = it - i + 1;
					res = max(res, util(boxes, i + 1, it - 1, 0) + util(boxes, it, j, k + 1));
				}
			}
			
			return dp[i][j][k] = res;
			
		}
	};

```

## Space and Time Complexity

* Space Complexity - O(1) - constant because irrespective of input length the space is 100x100x100
* Time Complexity - O(100x100x100) - In worst case we do N^3 recursions 

** Any updates to the complexity or problem solution is appreciated