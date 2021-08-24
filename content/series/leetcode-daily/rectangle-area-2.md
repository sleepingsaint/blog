---
title: "Rectangle Area 2"
date: 2021-08-24T14:50:29+05:30
draft: false
tags: ["line sweep", "merging intervals"]
series: "leetcode-daily"
---

## Question

We are given a list of (axis-aligned) rectangles. Each rectangle[i] = [xi1, yi1, xi2, yi2] , where (xi1, yi1) are the coordinates of the bottom-left corner, and (xi2, yi2) are the coordinates of the top-right corner of the ith rectangle.

Find the total area covered by all rectangles in the plane. Since the answer may be too large, return it modulo 10^9 + 7.

## Examples

<div >
<img src="example_1.png" style="width: 80%; background: salmon" />
</div>

Input: rectangles = [[0,0,2,2],[1,0,2,3],[1,0,3,1]] <br />
Output: 6 <br />
Explanation: As illustrated in the picture.
<hr />

Input: rectangles = [[0,0,1000000000,1000000000]] <br />
Output: 49 <br />
Explanation: The answer is 1018 modulo (109 + 7), which is (109)2 = (-7)2 = 49.
 
## Constraints

* 1 <= rectangles.length <= 200
* rectanges[i].length = 4
* 0 <= rectangles[i][j] <= 10^9
* The total area covered by all rectangles will never exceed 2^63 - 1 and thus will fit in a 64-bit signed integer.

## Explanation

First idea which strikes is to maintain a matrix which represents a grid and marks all the cells in the given rectangles and finally calculate the answer. This is a decent approach if the grid is small, but given the constraints it will only raise memory exceeded and time limit exceeded errors.

So we need to think a better approach. Now if you look closely to find the area of a certain portion like [1, 2] if we know the total height or space covered by the rectangles the area in that region will (2 - 1) * (space covered in the region). So now the problem is how to iterate over the regions and how to calculate the space (height) in the region.

If we start iterating, from the 0 to the 10^9 we make a lot of redundant calculations, instead we can iterate over the ranges where the rectangles exists. This is will save a lot of computation time. Now how to measure the height, for this we can store a hashmap to store all the rectangles in the region.

While iterating if we encounter a closing range for a rectangle we remove the rectangle from the hashmap, similarly if we encounter a starting range for a rectangle we add the rectangle to the hashmap. So to iterate the range we create a array of the opening and closing range of the rectangles and sort it in increasing order of the range positions, so that we don't miss any rectangles and also don't duplicate areas.

__Note__: If we encounter a case where at a point we have both closing range of a rectangle and opening range of another rectangle we need to first remove the previous rectangle so that we don't calculate the area of the previous rectangle again. 


## Code

```cpp

	typedef vector<vector<int>> vvi;
	typedef pair<int, int> pii;
	typedef vector<int> vi;

	#define mod 1000000007

	class Solution {
	public:
		long long calcArea(int width, map<pii, int> &intervals){
			long long area = 0;

			// this variable maintains the previous rectangle top
			long long prevTop = 0;
			
			// calculating the total height / area
			// of the merged intervals
			for(auto it: intervals){
				if(it.second == 0)
					continue;
				pair<int, int> interval = it.first;
				long long bottom = interval.first, top = interval.second;
				bottom = max(bottom, prevBottom);

				// if the bottom is still less then the current top
				// then we add it to the area and make the current top
				// as prevTop
				if(top > bottom){
					area += (top - bottom) * width;
					prevBottom = top;
				}
			}
			
			return area;
		}
		int rectangleArea(vector<vector<int>>& rectangles) {
			// if there are no rectangles then
			// area enclosed will be zero
			if(rectangles.empty())
				return 0;
			
			// creating an array to iterate over the 
			// range which all the rectangles exists 

			// I assumed the opening range and closing range as events 
			// and created a events array to store the events
			vvi events;
			for(vi &rec: rectangles){
				// adding 0 for the closing event so that 
				// after sorting we access the closing event first
				events.push_back(vi{rec[0], 1, rec[1], rec[3]});
				events.push_back(vi{rec[2], 0, rec[1], rec[3]});
			}
			
			// sorting the array so that we visit
			// the grid in an increasing range
			// and also we encounter the closing range of a rectangle first
			sort(events.begin(), events.end());

			long long area = 0;
			
			// hashmap to store the rectangles in the current range
			map<pair<int, int>, int> intervals;
			
			// this variable maintains the prev xposition of the current range
			int prev = 0;
			
			// iterating over the range
			for(vi &event: events){
				// calculating the area of the current region [prev, event[0]]
				area += calcArea(event[0] - prev, intervals);
				
				// if we encounter a starting range of rectangle
				// we add it to the intervals hashmap else
				// remove the rectangle hashmap
				if(event[1] == 1){
					intervals[{event[2], event[3]}]++;
				}else intervals[{event[2], event[3]}]--;
				
				// since we already calculated the current range area
				// we calculate the area of the next range by updating the
				// previous x position to current x position 
				prev = event[0];
			}

			// returning the area
			return area % mod;
		}
	};

```

## Space and Time Complexity

N - number of Rectangles

* Space Complexity: O(2 * N + N) - 2*N for storing the events array and N for storing the rectangles array
* Time Complexity: O(2N * NlogN + 2Nlog2N) - 2Nlog2N for sorting the events array, and 2N*NlogN because we iterating over each event and for every insert and remove operation takes logN time and N time for calculating the area of the merged intervals.