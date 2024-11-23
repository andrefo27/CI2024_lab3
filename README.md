
# Lab3 - N-Puzzle

## Objective
Solve efficiently a generic N²-1 puzzle (also known as Mystic Square, Gem Puzzle, Boss Puzzle).

### Evaluation Metrics
- **Quality**: Number of actions in the solution.
- **Cost**: Total number of actions evaluated.
- **Efficiency**: Quality vs. Cost ratio.

## Algorithm

The algorithm evaluates nodes in the frontier:
1. Pops the state with the smallest `f` value.
2. Checks if the state is the goal; if so, it reconstructs and returns the solution path.
3. Otherwise, it generates all possible neighbors (valid moves).
4. For each valid neighbor:
    - Calculates a new `f` value using the adaptive heuristic.
    - Adds it to the frontier unless already visited.

### Adaptive Behavior
The `magic_number` starts at a high value (`1.125 * n**(n-1)`), favoring exploration initially. 
On each iteration, it decays by a factor (`adaptive_factor = 0.985`) until it reaches the threshold (`0.0785`). 
This gradual decay shifts the algorithm's behavior:
- **Early search**: Encourages broader exploration to escape local minima or find diverse paths.
- **Late search**: Focuses more on exploitation to efficiently converge to the goal.

### Heuristic Switching
The `magic_number` determines the probability of switching heuristics between exploration and exploitation:
- **Exploration**: Uses the `misplaced_tiles` heuristic, favoring new states even if they might not seem immediately optimal.
- **Exploitation**: Uses the `linear_conflict_manhattan` heuristic, which is more informed and typically leads toward the goal.

## Blank Tile Representation
The blank tile in the puzzle is represented as **-1**.

## Heuristics
Useful resources for understanding and implementing A* heuristics:
- [A* Heuristics Implementation Guide](https://algorithmsinsight.wordpress.com/graph-theory-2/a-star-in-general/implementing-a-star-to-solve-n-puzzle/)

## Execution

Execution can start from:
1. A **random matrix**.
2. A **matrix defined by the user**.

### Random Matrix Generation
Two methods for generating a random matrix are available:
1. **Totally Random**: Uses a seed to ensure deterministic computation across different executions.
2. **Controlled Randomness**: The matrix is generated from the goal state with a known number of permutations (called `steps`), fixed a priori. This allows evaluation of how the algorithm behaves as the complexity of the matrix increases.

### Solvability
**Why is Solvability Important?**
- Without a solvable initial state, the goal state is unreachable.
- Solvability is an invariable property:
    - Every legal move (swapping the empty tile with an adjacent tile) keeps the puzzle in a solvable state.
    - There is no need to recheck solvability during the execution of A*, as every new configuration generated will automatically be solvable.

For more details on solvability, refer to this resource: [Check if an N-Puzzle is Solvable](https://www.geeksforgeeks.org/check-instance-15-puzzle-solvable/).

## Results

I am aware that the implemented algorithm does not perform efficiently as the size of N increases. However, it achieves good results, as far as i tried, with **4x4 matrices** generated with maximum 1000 steps.
With **5x5 matrices**, results can be obtained in reasonable times (less than a minute), but that is not guaranteed. To achieve that i suggest to not go over 200 random steps. Excessive steps may cause the algorithm to take unreasonable amounts of time and consume excessive memory, slowing down or even halting execution.
The most important thing i noticed is that the result is some kind related to the initial configuration of the matrix. So it really matters how you build it at the starting point of the algorithm and affect the times the algorthm takes to reach the goal state.

### Memory Usage
Be cautious of **RAM consumption**, as all explored nodes are stored in a set called `visited`. This can lead to high memory usage.

---

`gem-puzzle_basic.ipynb` does not have the `magic number` and the `adaptive factor`, and it uses `linear_conflict_manhattan` only. Meanwhile `gem-puzzle_adaptive.ipynb` should be the more optimized, with the `magic number` and the selection criteria for the heuristic.
