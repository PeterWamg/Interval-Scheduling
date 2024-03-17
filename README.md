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

### Counterexample for shortest interval

### Counterexample for fewest conflicts

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


## Algorithm application



## Algorithm implement
The Implementation of Algorithms in C++
```


```

## Time complexity




