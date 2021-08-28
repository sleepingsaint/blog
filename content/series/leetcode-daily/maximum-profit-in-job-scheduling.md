---
title: "Maximum Profit in Job Scheduling"
date: 2021-08-29T01:09:02+05:30
draft: false 
tags: ["array", "dynammic programming", "sorting"]
series: "leetcode-daily" 
---

## Question

We have n jobs, where every job is scheduled to be done from startTime[i] to endTime[i], obtaining a profit of profit[i].

You're given the startTime, endTime and profit arrays, return the maximum profit you can take such that there are no two jobs in the subset with overlapping time range.

If you choose a job that ends at time X you will be able to start another job that starts at time X.

## Examples

<img src="example1.png">

Input: startTime = [1,2,3,3], endTime = [3,4,5,6], profit = [50,10,40,70] <br />
Output: 120 <br />
Explanation: The subset chosen is the first and fourth job. 
Time range [1-3]+[3-6] , we get profit of 120 = 50 + 70.
<hr />

<img src="example2.png">

Input: startTime = [1,2,3,4,6], endTime = [3,5,10,6,9], profit = [20,20,100,70,60] <br />
Output: 150 <br />
Explanation: The subset chosen is the first, fourth and fifth job. 
Profit obtained 150 = 20 + 70 + 60.
<hr />

<img src="example3.png">

Input: startTime = [1,1,1], endTime = [2,3,4], profit = [5,6,4] <br />
Output: 6

## Constraints

* 1 <= startTime.length == endTime.length == profit.length <= 5 * 104
* 1 <= startTime[i] < endTime[i] <= 109
* 1 <= profit[i] <= 104

## Explanation

For the given jobs, we might pick a job or we don't. If we picked a job, then we need to pick only those jobs which have starting times, greater than or equal to the picked job finish time. Since the given parameters are not in a certain, we __sort__ the jobs based on the starting times of a job, so that if we need to look for the jobs which have greater starting time than the current picked job's ending time, we can be sure that the jobs before the current node are pickable. 

We use __recursion__ to calculate the answer. Every recursive call calculates the maximum profit for the jobs from current job to last job. Using recursion we will calling multiple recursive calls, to avoid that we can use a __memoization / dynammic programming__ to store the result for the given range of jobs.

## Code

```cpp

	// data structure to represt the job
	struct Job{
		int start, finish, profit;
	};

	class Solution {
	public:
		int jobScheduling(vector<int>& startTime, vector<int>& endTime, vector<int>& profit) {
			int n = startTime.size();
			
			// creating the jobs using the defined Job class
			vector<Job> jobs;
			for(int i = 0; i < n; i++){
				jobs.push_back({startTime[i], endTime[i], profit[i]});
			}
			
			// sorting the jobs in the order of start times
			sort(jobs.begin(), jobs.end(), [](Job &j1, Job &j2){
				return j1.start < j2.start;
			});
			
			// dp array to memoize the results
			vector<int> dp(n, -1);

			// calling the recursive function
			return solve(jobs, dp, 0);
		}
		
		int solve(vector<Job> &jobs, vector<int> &dp, int idx){
			// if we picked all the jobs 
			// we got
			if(idx == jobs.size())
				return 0;
			
			// if the value is already calculated
			// then return the calculated result
			if(dp[idx] != -1)
				return dp[idx];
			
			// this stores the result if we dont pick the job
			int exclude = solve(jobs, dp, idx + 1);

			// this stores the result if we pick the current job
			int include = jobs[idx].profit;
			
			// calculating the index of next valid job 
			// whose start time is more than or equal to current job finish time
			int i = idx + 1;
			while(i < jobs.size()){
				if(jobs[i].start >= jobs[idx].finish)
					break;
				i++;
			}
			
			// if there exists a valid job
			// add the result to the current included result
			if(i != jobs.size())
				include += solve(jobs, dp, i);
			
			// return maximum of included and excluded results
			return dp[idx] = max(exclude, include);
		}
	};

```

## Space and Time Complexity

N - Number of jobs

* Space Complexity: O(N + N + N) - one N for the jobs array, another N for the dp array, another for the recursion stack - in worst case the maximum depth would be N
* Time Complexity: O(N + NlogN + NxN) -  N for creating the jobs array, NlogN for sorting the jobs array, and in NxN, one N is for, for every job, we are visiting it only once, since we are storing the result in the dp table, and another N is for finding the next valid job which can take N iterations in worst case.