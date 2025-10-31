Name : NISHANTH S
Reg No : 212224060175

# ExpNo:10 Implementation of Classical Planning Algorithm
# Algorithm or Steps Involved:
<ol>
  <li>Define the initial state</li>
  <li>Define the goal state</li>
  <li>Define the actions</li>
  <li>Find a <b>plan</b> to reach the goal state</li>
  <li>Print the plan</li>
</ol>

# Example - 1
```
initial_state = {'A': 'Table', 'B': 'Table'}
goal_state = {'A': 'B', 'B': 'Table'}

actions = {
    'move_A_to_B': {'precondition': {'A': 'Table', 'B': 'Table'}, 'effect': {'A': 'B'}},
    'move_B_to_Table': {'precondition': {'A': 'Table', 'B': 'B'}, 'effect': {'B': 'Table'}}
}

plan = find_plan(initial_state, goal_state, actions)
print(plan)
```
# Output:
```
['move_A_to_B']
```
# Example - 2
```
initial_state = {'A': 'Table', 'B': 'Table', 'C': 'Table'}
goal_state = {'A': 'B', 'B': 'C', 'C': 'Table'}

actions = {
    'move_A_to_B': {'precondition': {'A': 'Table', 'B': 'Table'}, 'effect': {'A': 'B'}},
    'move_B_to_C': {'precondition': {'A': 'B', 'B': 'Table', 'C': 'Table'}, 'effect': {'B': 'C'}},
    'move_C_to_Table': {'precondition': {'A': 'B', 'B': 'C', 'C': 'C'}, 'effect': {'C': 'Table'}}
}

plan = find_plan(initial_state, goal_state, actions)
print(plan)
```
# Output:
```
['move_A_to_B', 'move_B_to_C']
```

# Please Prepare Solution or Definition For the method find_plan(initial_state, goal_state, actions)
<h3>You Can use any of the searching Strategies for planning and executing a sequence of actions.<br> You can also look in to the Code given in the Repository.</h3>
# Classical Planning using Search Algorithm (Forward Planning)

from collections import deque

def is_goal_state(state, goal_state):
    """Check if the current state satisfies the goal state."""
    for key in goal_state:
        if key not in state or state[key] != goal_state[key]:
            return False
    return True


def applicable(action, state, actions):
    """Check if an action can be applied in the current state."""
    precond = actions[action]['precondition']
    for key, val in precond.items():
        if key not in state or state[key] != val:
            return False
    return True


def apply_action(state, action, actions):
    """Apply an action to generate a new state."""
    new_state = state.copy()
    effects = actions[action]['effect']
    for key, val in effects.items():
        new_state[key] = val
    return new_state


def find_plan(initial_state, goal_state, actions):
    """Find a sequence of actions (plan) to reach goal using BFS search."""
    frontier = deque([(initial_state, [])])  # (state, plan)
    explored = set()

    while frontier:
        state, plan = frontier.popleft()

        # Convert state to tuple for hashing
        state_tuple = tuple(sorted(state.items()))
        if state_tuple in explored:
            continue
        explored.add(state_tuple)

        # Check if goal achieved
        if is_goal_state(state, goal_state):
            return plan

        # Try applicable actions
        for action in actions:
            if applicable(action, state, actions):
                new_state = apply_action(state, action, actions)
                new_plan = plan + [action]
                frontier.append((new_state, new_plan))

    return None


# -------------------- Example 1 --------------------
print("Example 1:")
initial_state = {'A': 'Table', 'B': 'Table'}
goal_state = {'A': 'B', 'B': 'Table'}

actions = {
    'move_A_to_B': {'precondition': {'A': 'Table', 'B': 'Table'}, 'effect': {'A': 'B'}},
    'move_B_to_Table': {'precondition': {'A': 'Table', 'B': 'B'}, 'effect': {'B': 'Table'}}
}

plan = find_plan(initial_state, goal_state, actions)
print("Plan:", plan)
print()


# -------------------- Example 2 --------------------
print("Example 2:")
initial_state = {'A': 'Table', 'B': 'Table', 'C': 'Table'}
goal_state = {'A': 'B', 'B': 'C', 'C': 'Table'}

actions = {
    'move_A_to_B': {'precondition': {'A': 'Table', 'B': 'Table'}, 'effect': {'A': 'B'}},
    'move_B_to_C': {'precondition': {'A': 'B', 'B': 'Table', 'C': 'Table'}, 'effect': {'B': 'C'}},
    'move_C_to_Table': {'precondition': {'A': 'B', 'B': 'C', 'C': 'C'}, 'effect': {'C': 'Table'}}
}

plan = find_plan(initial_state, goal_state, actions)
print("Plan:", plan)
