---
title: "Complex Number Multiplication"
date: 2021-08-24T13:58:03+05:30
draft: false 
tags: ["string", "parsing"]
series: "leetcode-daily" 
---

## Question

A complex number can be represented as a string on the form "real+imaginaryi" where:

* real is the real part and is an integer in the range [-100, 100].
* imaginary is the imaginary part and is an integer in the range [-100, 100].
* i^2 == -1.

Given two complex numbers num1 and num2 as strings, return a string of the complex number that represents their multiplications.

## Examples

Input: num1 = "1+1i", num2 = "1+1i"<br />
Output: "0+2i"<br />
Explanation: (1 + i) * (1 + i) = 1 + i2 + 2 * i = 2i, and you need convert it to the form of 0+2i.
<hr />

Input: num1 = "1+-1i", num2 = "1+-1i"<br />
Output: "0+-2i"<br />
Explanation: (1 - i) * (1 - i) = 1 + i2 - 2 * i = -2i, and you need convert it to the form of 0+-2i.

## Constraints

* num1 and num2 are valid complex numbers.

## Explanation

The solution for this problem is very straight forward. We need to parse the real and imaginary parts of the given numbers and then perform the multiplication and convert the result back to the answer. The real trick is to know how to convert the string to integer and vice versa.

## Code

```cpp

	class Solution {
	public:
		string complexNumberMultiply(string num1, string num2) {
			// calculating the integer representation of the numbers
			vector<int> n1 = getNum(num1), n2 = getNum(num2);

			// calculating the real, imaginary parts of the result
			int real = n1[0]*n2[0] - (n1[1]*n2[1]);
			int complex = n1[1]*n2[0] + n1[0]*n2[1];

			// converting the result to the string format
			string res = to_string(real) + "+" + to_string(complex) + "i";
			return res;
		}
		
		// converting the complex number in string form 
		// to real and imaginary components and returning
		// in vector<int>{real, imaginary} format
		vector<int> getNum(string num){
			vector<int> res;
			
			int n = num.length(), i = 0, start = 0;
			
			while(i < n){
				// if we encounter the +, i characters then 
				// we found the real and imaginary parts
				// hence we parse the corresponding substring
				// and add it to the answer
				if(num[i] == '+' || num[i] == 'i'){
					res.push_back(stoi(num.substr(start, i - start)));
					start = i + 1;
				}
				i++;
			}
			
			return res;
		}
	};

```

## Space and Time Complexity

* Space Complexity: O(1) - Since no matter what the input is we would store two vectors of length 2
* Time Complexity: O(n) - where n is the maximum of num1, num2 string lengths, since we are calculating the real, complex parts of the number