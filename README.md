# Interval-Scheduling

## Introduction
Interval Scheduling is an optimization problem-solving method commonly used for resource scheduling or task scheduling.

## Description
Interval Scheduling is Greedy algorithm that selects as many tasks as possible without generating conflicts, given a series of tasks and their time intervals. Each task has a start and end time, and there may be conflicts or overlapping times between tasks.

![Image Title](001.png)

## Gold
Find maximum subset of mutually compatible j

## Greedy template

### Counterexample for earliest start time
![Image Title](01.png)

### Counterexample for shortest interval
![Image Title](02.png)

### Counterexample for fewest conflicts
![Image Title](03.png)

## Pseudocode

```
Interval-Scheduling(intervals):
    Sort intervals by finish time
    selected_intervals = []
    last_end_time = -∞

    for interval in intervals:
        if interval's start time ≥ last_end_time:
            Add interval to selected_intervals
            last_end_time = interval's end time

    return selected_intervals

```
### Algorithmic principle

Interval-Scheduling algorithm employs a greedy strategy, sorting intervals by finish times and iteratively selecting non-overlapping intervals with earliest finish times, ensuring maximum interval coverage without conflicts.

## Operate

- Sort the task list by end time from morning to evening.
- Initialize an empty list selected_intervals to store the selected tasks.
- Initialize the variable last_end_time to negative infinity.
- Traversing the sorted task list:
      - For each task, check if its start time is later than or equal to last_end_time:
          - If so, add the task to selected_intervals and update the last end time to the end time of the task.
          - If not, ignore the task.
- Return the selected_intervals list, which contains as many tasks as possible without conflicts.





## Algorithm implement
The Implementation of Algorithms in C++
```
int intervalSchedule(std::vector<std::vector<int>>& intervals) {
        if (intervals.empty()) return 0;
        // Sort by end time in ascending order
        std::sort(intervals.begin(), intervals.end(), [](const std::vector<int>& a, const std::vector<int>& b) {
            return a[1] < b[1];
        });
        // At least one interval is non-overlapping
        int count = 1;
        // The first interval after sorting is considered
        int x_end = intervals[0][1];
        for (const auto& interval : intervals) {
            int start = interval[0];
            if (start >= x_end) {
                // Found the next non-overlapping interval
                count++;
                x_end = interval[1];
            }
        }
        return count;
    }

```

## Time complexity
In this algorithm, the time complexity is O (n log n), where n is the number of tasks. The subsequent traversal operation is linear with a time complexity of O (n). Therefore, the time complexity of the entire algorithm is O (n log n).

## Algorithm application
The Interval Scheduling algorithm is mostly used in time management in practical life, such as airlines arranging flights and arranging takeoff and landing times reasonably to ensure sufficient time intervals between flights, while minimizing aircraft waiting time and ground congestion.
>
Alternatively, by optimizing the use of classrooms, schools can maximize the utilization of teaching resources and ensure that there are no time conflicts between courses


# Weighted Interval Scheduling

## Introduction
Weighted Interval Scheduling is an extension of the Interval Scheduling problem, which considers not only the start and end times of tasks but also their associated weights or values.

## Description
In Weighted Interval Scheduling, each task has a start time, an end time, and a weight, representing its importance or value. The goal is to select a subset of tasks such that no two tasks overlap in time and the total weight of the selected tasks is maximized. 

## Goal
Find an optimal solution that balances task selection and time constraints to achieve the highest possible overall value.




## Pseudocode

```

Input: n, s1,...,sn , f1,...,fn , v1,...,vn
Sort jobs by finish times so that f1 ≤ f2 ≤ ... ≤ fn.
Compute p(1), p(2), ..., p(n)
Compute-Opt(j) {
    if (j = 0)
        return 0
    else
        return max(vj + Compute-Opt(p(j)), Compute-Opt(j-1))
}
Weighted Interval Scheduling: Brute Force
Brute force algorithm.



```
### Algorithmic principle
Calculate the maximum total value by recursively considering two options for each task (including or not including) and selecting the optimal solution. By continuously considering the compatibility and value of tasks, we ultimately find the optimal combination of tasks to achieve maximum total value.

## Operate

- Firstly, sort all tasks by end time to ensure that they are arranged in non decreasing order of end time.
- For each task j, find the index of the last completed task before the start of task j. This index represents the last compatible task of task j, denoted as p (j).
- Use the recursive function Compute Opt (j) to calculate the maximum value. This function takes task index j as a parameter and returns the maximum total value that can be obtained from task 1 to task j.
          - Basic situation: If j=0, there are no tasks to choose from, and the return value is 0.
          - Recursive situation: For task j, there are two options: Including task j: At this point, the total value is vj plus the maximum total value opened by p (j)
                - Including task j: At this point, the total value is vj plus the maximum total value opened by p (j)
                - Excluding task j: At this point, the total value ranges from 1 to j-1, indicating one's maximum total value
  - the calculation result is the maximum value between these two options.

- Call the Compute-Opt function with n as a parameter to find the maximum total value considering all tasks.

## Algorithm implement
The Implementation of Algorithms in C++
```
bool compareJobs(const Job& a, const Job& b) {
    return a.finish < b.finish;
}

int findLastCompatible(const vector<Job>& jobs, int currentJob) {
    for (int i = currentJob - 1; i >= 0; --i) {
        if (jobs[i].finish <= jobs[currentJob].start) {
            return i;
        }
    }
    return -1;  // No compatible job found
}

int computeOpt(const vector<Job>& jobs, vector<int>& memo, int currentJob) {
    if (currentJob == 0) {
        return 0;
    }
    if (memo[currentJob] != -1) {
        return memo[currentJob];
    }
    int lastCompatible = findLastCompatible(jobs, currentJob);
    int includeCurrent = jobs[currentJob].value + (lastCompatible == -1 ? 0 : computeOpt(jobs, memo, lastCompatible));
    int excludeCurrent = computeOpt(jobs, memo, currentJob - 1);
    memo[currentJob] = max(includeCurrent, excludeCurrent);
    return memo[currentJob];
}

int weightedIntervalScheduling(vector<Job>& jobs) {
    sort(jobs.begin(), jobs.end(), compareJobs);
    vector<int> memo(jobs.size(), -1);
    return computeOpt(jobs, memo, jobs.size() - 1);
}

```

## Time complexity 

The time complexity of sorting tasks by end time is O (n log n), where n is the number of tasks.
For each task j, it is necessary to find the index of the last compatible task and calculate the maximum value of the two choices. In the worst-case scenario, for each task j, it is necessary to traverse the previous tasks to find the last compatible task, resulting in a total time complexity of O (n<sup>2</sup>). However, due to the use of memory search and the avoidance of duplicate calculations, the actual time complexity will be lower, approaching linear complexity.
Overall the time complexity of the algorithm is O (n log n), where n is the number of tasks.


# Interval Partitioning

## Introduction
Interval Partitioning is a problem-solving technique used to assign a series of tasks (or activities) with start and end times to a set of resources in order to minimize the number of resources

## Description
Arrange a series of meetings in the minimum number of meeting rooms to ensure that there are no conflicts in meeting time. The key to solving this problem lies in scheduling meetings reasonably to minimize the use of conference room resources while ensuring that all meetings can be held smoothly.

## Goal
Ensure that each task is assigned to a resource and there are no conflicts between resources, i.e. the time intervals of the tasks do not overlap.

![Image Title](04.png)


## Pseudocode

```

Sort intervals by starting time so that s1 ≤ s2 ≤ ... ≤ sn.
d ← 0
for j ← 1 to n {
    if (lecture j is compatible with some classroom k)
        schedule lecture j in classroom k
    else
        allocate a new classroom d + 1
        schedule lecture j in classroom d + 1
        d ← d + 1
}
return number of allocated classrooms


```
### Algorithmic principle
Sort tasks by start time and schedule them within the same resource (such as a classroom) as much as possible until time conflicts arise. If there is a time conflict, allocate a new resource and schedule the task within the new resource.

## Operate

- Firstly, sort the tasks according to their start time to ensure they are processed in chronological order.
- Initialize a variable (such as classrooms) to record the number of assigned classrooms, with an initial value of 0.
- Traverse the sorted task list and process each task as follows:
          - Check if the current task is compatible with an assigned classroom (i.e. does not conflict with any tasks in the assigned classroom).
          - If the current task is compatible with a certain classroom, schedule the task in that classroom.
          - If the current task is not compatible with all assigned classrooms, assign a new classroom with the number classrooms+1 to the current task.
- Every time a new classroom is assigned to a task, increase the number of assigned classrooms by 1.
- Finally, return the allocated number of classrooms, which is the minimum required number of classrooms.


## Algorithm implement
The Implementation of Algorithms in C++
```
int intervalPartitioning(std::vector<Interval>& intervals) {
    if (intervals.empty()) return 0;
    std::sort(intervals.begin(), intervals.end(), compareStartTime);
    std::vector<int> endTimes;
    int allocatedClassrooms = 0;
    for (const auto& interval : intervals) {
        bool assigned = false;
        for (int i = 0; i < endTimes.size(); ++i) {
            if (interval.start >= endTimes[i]) {
                endTimes[i] = interval.end;
                assigned = true;
                break;
            }
        }
        if (!assigned) {
            endTimes.push_back(interval.end);
            ++allocatedClassrooms;
        }
    }
    return allocatedClassrooms + 1;
}

```

## Time complexity 

The time complexity depends on sorting and traversal operations.
Sorting operation: Sort tasks according to their start time, with a time complexity of O (n log n), where n is the number of tasks.
Traversing operation: Traversing the sorted task list once, each task needs to search for available time periods in the assigned classrooms, with a time complexity of O (k), where k is the number of assigned classrooms. Due to the fact that each task needs to be compared to at most each time period in the assigned classroom, the overall time complexity is O (nk).
Overall, the time complexity of this code is O (n log n+nk). In the worst-case scenario, when k approaches n, the complexity can be simplified to O (n<sup>2</sup>).


# Scheduling to Minimize Lateness

## Introduction
Scheduling to Minimize Lateness is a problem-solving technique used in task management to minimize the lateness of completing tasks in relation to their respective deadlines. 

## Description
Each task is associated with a deadline by which it must be completed. The goal is to arrange the tasks in a schedule that minimizes the lateness of each task, which is defined as the amount of time by which the task completion exceeds its deadline. 

## Goal
Create a schedule that minimizes the lateness of completing tasks in relation to their deadlines.

![Image Title](245.png)


## Pseudocode

```

Sort n jobs by deadline so that d1 ≤ d2 ≤ ... ≤ dn
t ← 0
for j = 1 to n
    Assign job j to interval [t, t + tj]
    sj ← t, fj ← t + tj
    t ← t + tj
output intervals [sj, fj]



```
### Algorithmic principle
Sort tasks by deadline and arrange their execution order based on their execution time to minimize task latency. By greedily scheduling tasks according to their deadlines, it is possible to minimize task delays and improve task execution efficiency.

## Operate

- Firstly, sort the tasks according to their deadline d, ensuring that d1 ≤ d2 ≤... ≤ DN
- Initialize a time variable t to 0, representing the current point in time.
- Traverse the sorted task list, for each task:
          - Assign the current task j to the time period [t, t+tj] for execution, where tj is the execution time of task j
          - Record the start time sj of task j as the current time t, and the end time fj as the current time t plus the execution time tj
          - Update the current time t to the time point t+tj after the task execution ends, which is the time point at which the next task can start.
- Output the time period [sj, fj] for all tasks, representing the execution time period for each task.

## Algorithm implement
The Implementation of Algorithms in C++
```
void scheduleJobs(vector<Job>& jobs) {
    // Sort jobs by deadline
    sort(jobs.begin(), jobs.end(), compareDeadline);
    
    int currentTime = 0;
    vector<pair<int, int>> intervals;
    
    for (int i = 0; i < jobs.size(); ++i) {
        Job& currentJob = jobs[i];
        int startTime = currentTime;
        int endTime = currentTime + currentJob.duration;
        
        intervals.push_back({startTime, endTime});
        
        // Update current time
        currentTime += currentJob.duration;
    }
    
    // Output intervals
    for (int i = 0; i < intervals.size(); ++i) {
        cout << "Interval for job " << i + 1 << ": [" << intervals[i].first << ", " << intervals[i].second << "]" << endl;
    }
}

```

## Time complexity 

The time complexity of this algorithm is O (n log n), where n is the number of tasks, as it involves sorting tasks by deadline and then linearly scanning the sorted task list to allocate execution time slots for each task.














