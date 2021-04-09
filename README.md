# Accumulated Times

## Problem Statement

A dasher is dropping off orders in a large building and we want to
 compute how much time they spend in each room. We are given a list of elements containing the name of a room, a time period, and a value
indicating whether a user is entering/leaving the room. The dasher cannot be in more than one room
at the same time. Once they exit a room, they return to the previous room. Determine the time spent in each room.
i.e.: 
```
Lobby 0 Enter
Hallway1 10 Enter
Room1 30 Enter
Room1 40 Exit // back in hallway1 now
Room1 50 Enter
Room1 60 Exit
Hallway1 90 Exit // back in Lobby
Hallway2 100 Enter
Hallway2 150 Exit
Lobby 160 Exit
```

Output:
```
map(Lobby = 30, Hallway1 = 60, Room1 = 20, Hallway2 = 50)
```

Allow the interviewee to construct their own data structures for function input/output.
Gain some understanding of whether they are comfortable in their language of choice and if they have
some reasoning for their choice.
Some examples:
```
Kotlin - List<Data class(enum, int, boolean/enum)> for cleanliness, readability, and unit testability
Python - List<Tuple> for ease of looping through
```

### Constraints

Candidates should ask for these themselves in order to fully grasp the problem.
If these are given by the interviewer or not considered in any way, reflect in scorecard below.

- Elements are sorted with respect to time.
- No negative times.
- Can assume valid flow. Each entered room will have an exit.
- Can assume non-cyclical/non-recursive flows. A dasher can't re-enter the room 
they're already in and they won't re-enter a room they've already entered without exiting. (i.e. Once in Hallway1, can't enter Lobby)

## Interviewer Notes

### Solution
The proposed solution uses a stack to maintain a memory of which room the dasher is currently in.
We then keep track of how much time they've spent in the room since the last action and we can increment
our returned data structure accordingly.

#### Things to Consider

There are plenty of solutions, however a stack approach is intuitive and efficient.

Don't reveal the constraints above and see if the interviewee clears up ambiguity and thinks through potential edge cases.

For hints, can also provide the constraints above.

### Extension 1

What if we can be given a sequence where not every begin has a matching end and we want to return null/empty in these cases?
i.e.
```
Enter lobby
Enter floor 1
Enter hallway 1
Exit hallway 1
Exit lobby
```

We can expand on this case shown in the solution.
```
if (!currentElement.entered) {
    stack.pop()
} else {
    stack.push(currentElement.room)
}
```

And change this to: 
```
if (!currentElement.entered) {
    if (currentElement.room != peakedElement) {
        return emptyMap()
    }
    stack.pop()
} else {
    stack.push(currentElement.room)
}
```

In addition we need to account for the case where we have an incomplete begin statement.
This can be done near the end with an empty check on the stack: 
```
if (!stack.isEmpty()) {
    return emptyMap()
}
```

See `extended_solution.kt`.
### Extension 2

Design unit tests for given function. Look for full branch coverage at the very least. 
All code flows should be tested. Bonuses for boundary test cases and more bad data test cases (cases looking to
break the function which can help highlight missed constraints above). 

This extension highlights a candidate's ability to reason through test cases as well as
to gauge whether they've missed any edge cases/assumptions. 

### Evaluation

The candidate should be able to solve the base problem and extension unit test cases in the given time with minimal help.

Rubric:
```
Inputs (0-1):
- Reasonable input: 1
- Little thought given to input which leads to unnecessary complication of algorithm: 0

Clearing ambiguity (0-2):
- Asks about at least 3 of the constraints: 2
- Asks about at least 2 of the constraints: 1
- Else: 0

Actual execution/coding of problem (0-10):
- No help needed, able to design efficient solution: 10
- Minimal help needed: 8-9
- Some help needed, particularly in getting started: 5-7
- Significant help needed and/or answer not finished completely: 0-4

Extension 1 (0-3):
- Fully able to solve extension (all cases) without any help: 3
- Missing/help needed with the non-empty stack check: 2
- Missing/help needed with the in-loop check: 1
- Missing both checks: 0

Extension 2 (0-3): 
- Well thought out unit tests with all branch/empty cases as well as consideration for boundary and edge cases. Deep understanding of industry standard unit testing practices: 3
- Clear thought process used in unit tests. At least covers all branch statements including empty/null cases: 2
- Missed cases. Little/no logic used when designing tests. Random approach rather than making tests based on the code itself: 0-1

Recommended scorecard:
    Strong yes: 18-19
    Yes: 15-17
    No: 7-14
    Strong no: 0-6
```
