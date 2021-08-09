---
title: "Add Strings"
date: 2021-08-09T14:48:21+05:30
draft: false 
tags: ["string"]
series: "leetcode-daily" 
---

## Question

Given two non-negative integers, num1 and num2 represented as string, return the sum of num1 and num2 as a string.

You must solve the problem without using any built-in library for handling large integers (such as BigInteger). You must also not convert the inputs to integers directly.

## Examples

Input: num1 = "11", num2 = "123"

Output: "134"
<hr />

Input: num1 = "456", num2 = "77"

Output: "533"
<hr />

Input: num1 = "0", num2 = "0"

Output: "0"
<hr />

## Constraints

* 1 <= num1.length, num2.length <= 104
* num1 and num2 consist of only digits.
* num1 and num2 don't have any leading zeros except for the zero itself.

## Explanation

This problem can be solved using the approach we used to, when we are learning additions. We start from the unit digits or last digits and add them up and store the carry and then move on to next digits and so on. The catch here is, we can't add the characters directly so we convert them to integers and add them and convert the result to character and insert it in the answer.

## Code

```cpp

	class Solution {
	public:
		string addStrings(string num1, string num2) {
			string str = "";
			int n1 = num1.length(), n2 = num2.length();
			
			// variables to maintain the current digits of 
			// each number
			int i = n1 - 1, j = n2 - 1;
			int carry = 0;
			
			// iterating either till both numbers are completed
			// ie. both numbers are of same size
			// or alteast one number is completed
			while(i >= 0 && j >= 0){
				// converting the characters to integers
				int x = num1[i] - '0', y = num2[j] - '0';
				
				// getting the new result
				int sum = x + y + carry;
				carry = sum / 10;

				// converting the integer answer to character 
				char c = (sum % 10) + '0';
				str = c + str;

				// moving to next digits
				i--;
				j--;
			}
			
			// if the num1 have more digits than num2
			while(i >= 0){
				int x = num1[i] - '0';
				int sum = x + carry;
				carry = sum / 10;
				char c = (sum % 10) + '0';
				str = c + str;
				i--;
			}
			
			// if num2 have more digits than num1
			while(j >= 0){
				int y = num2[j] - '0';
				int sum = y + carry;
				carry = sum / 10;
				char c = (sum % 10) + '0';
				str = c + str;
				j--;
			}
			
			// if there is a carry we add it to the result
			if(carry != 0){
				char c = carry + '0';
				str = c + str;
			}
			
			return str;
		}
	};

```

## Space and Time Complexity

* Space Complexity: O(1) - since no variable is dependent on input size except the result which we can neglect
* Time Complexity: O(max(n1, n2)) - In the worst case, the max time will be due to the number with more digits