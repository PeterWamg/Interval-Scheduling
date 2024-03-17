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














