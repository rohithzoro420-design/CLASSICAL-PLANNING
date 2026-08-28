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

program

~~~

from collections import deque

def satisfies(state, condition):
    for key, value in condition.items():
        if state.get(key) != value:
            return False
    return True

def apply_action(state, effect):
    new_state = state.copy()
    new_state.update(effect)
    return new_state

def find_plan(initial_state, goal_state, actions):
    queue = deque()
    queue.append((initial_state, []))
    visited = set()

    while queue:
        current_state, plan = queue.popleft()

        state_key = tuple(sorted(current_state.items()))

        if state_key in visited:
            continue

        visited.add(state_key)

        # Goal Test
        if satisfies(current_state, goal_state):
            return plan

        # Expand all applicable actions
        for action_name, action in actions.items():
            if satisfies(current_state, action["precondition"]):
                next_state = apply_action(current_state, action["effect"])
                queue.append((next_state, plan + [action_name]))

    return None


print("Example 1")

initial_state = {
    'A': 'Table',
    'B': 'Table'
}

goal_state = {
    'A': 'B',
    'B': 'Table'
}

actions = {
    'move_A_to_B': {
        'precondition': {
            'A': 'Table',
            'B': 'Table'
        },
        'effect': {
            'A': 'B'
        }
    },

    'move_B_to_Table': {
        'precondition': {
            'A': 'Table',
            'B': 'B'
        },
        'effect': {
            'B': 'Table'
        }
    }
}

plan = find_plan(initial_state, goal_state, actions)

print("Plan:", plan)


print("\nExample 2")

initial_state = {
    'A': 'Table',
    'B': 'Table',
    'C': 'Table'
}

goal_state = {
    'A': 'B',
    'B': 'C',
    'C': 'Table'
}

actions = {
    'move_A_to_B': {
        'precondition': {
            'A': 'Table',
            'B': 'Table'
        },
        'effect': {
            'A': 'B'
        }
    },

    'move_B_to_C': {
        'precondition': {
            'A': 'B',
            'B': 'Table',
            'C': 'Table'
        },
        'effect': {
            'B': 'C'
        }
    },

    'move_C_to_Table': {
        'precondition': {
            'A': 'B',
            'B': 'C',
            'C': 'C'
        },
        'effect': {
            'C': 'Table'
        }
    }
}

plan = find_plan(initial_state, goal_state, actions)

print("Plan:", plan)

~~~

output

<img width="381" height="116" alt="image" src="https://github.com/user-attachments/assets/bc748e1b-5b21-4036-8b81-ae9064c51b92" />
