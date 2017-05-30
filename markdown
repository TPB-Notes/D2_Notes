# Decision 2 Notes

## Critical path analysis

### Activity networks

The aim of an activity network is to determine the minimum value, in time or resources, with which a project can be completed.

The network should be drawn such that preceding activities are all shown to the left of the given activity.
The activity name is written along with a number indicating the duration or cost of the activity.

 1. Draw a circle to represent the start of all those activities that have no immediate predecessors
 2. From the most recently drawn circles draw the start of arcs directed from left to right to represent the activities starting from those circles
 3. To the right of any previous drawing, draw circles to represent the start of all those activities for which all immediate predecessors have already been drawn. Two activities which have precisely the same immediate predecessors can share a start circle
 4. Join all loose arcs to relevant right hand circles, introducing dummy activities as necessary
 5. Repeat steps 2, 3, and 4 until all activities have been considered
 6. Join all loose arcs to a circle at the right hand side of the diagram, to represent the finish

#### Earliest and latest start times

The earliest time after the start of a project that an activity can be started is called the earliest start time.

The earliest start times are calculated by performing a forward pass, starting from the left.

 1. Label the initial node with earliest start time 0
 2. Choose any node, $X$, such that all nodes to its left have already been given an early time
 3. Consider all activities leading into $X$, and the nodes from which they come. Label $X$ with an earliest start time equal to the maximum value of the earliest start time of the preceding node plus the duration of the preceding activity for all these activities
 4. If all nodes have been labelled then stop. Otherwise, return to step 2

The latest start time for a node is the latest that activities to the left can finish and not delay the completion of the project.
Once the earliest start times have been determined, the latest start times can be found by performing a reverse pass from the right to the left.

 1. Label the final node with a latest start time equal to its earliest start time
 2. Choose any node, $X$, such that all nodes to its right have already been given a latest start time
 3. Consider all activities leading out of $X$. Label $X$ with a late time equal to the least value of the latest start time of the following node minus the duration of the following activity for all these activities
 4. If all nodes have been labelled then stop. Otherwise, return to step 2

### Critical paths

**Critical activity** 
Critical activities are such that they connect two nodes where both nodes have the same earliest start times and latest start times
Their duration is the difference between the labels of their left and right nodes
A critical activity has no scheduling flexibility

**Critical path** A path of critical activities leading from the start node to the final node

**Float** The amount of time by which an activity can be delayed or extended

**Independent float** A float which does not affect other activities


### Cascade diagrams

Once the earliest start times and latest end times have been calculated, a cascade chart provides a method of displaying information in a way which is easy to understand.

It is convention to place an activity at its earliest possible start time, and use dashed lines to display the possible range.

#### Resource leveling

The smoothing out of the usage of resources is called resource levelling.

Draw the cascade chart, starting with any critical paths along the bottom.
Then insert other activities to produce a chart of minimum height.

## Allocation with the Hungarian algorithm

**Assignment problem** Finding a maximum weight matching or minimum weight perfect matching in a weighted bipartite graph

If the matrix is not a square, add a dummy row or column filled with values which are equal to the maximum value in the matrix.

If the objective is to establish a maximal matching, subtract each of the values from the maximum value in the matrix.

 1. Reduce the array of costs by both row and column subtraction. This is subtracting the smallest value from every value in each row and column
 2. Cover the zero elements with the minimum number of lines. If this minimum number is the same as the size of the array, go to step 4.
 3. Let $m$ be the minimum uncovered element. Augment the array by reducing all uncovered elements by m and increasing all elements covered by two lines by $m$. Return to step 2.
 4. There is a maximal matching using only zeros. Apply this pattern to the original array.

## Dynamic programming

**States** Nodes are called states

**Stage** The transition from states with a stage variable $k$ to those with $k-1$ is called stage $k$

**Actions** Arcs are called actions. an action transforms a situation from one state to another

**Costs** The weight on an arc is called the cost

A general strategy can be used to solve optimisation problems. The principle of this strategy is to work backwards through the decisions, at each stage working out the best strategy from that point.

**Sub-optimal strategy** The best strategy from each point is called the sub-optimal strategy

### Maximin and minimax problems

**Maximin** A problem where the objective is to maximise the minimum value

**Minimax** A problem where the objective is to minimise the maximum value

#### Minimum
| Stage | State | From | Calculation | Value |
| --- | --- | --- | --- | --- |
| 1 | I | K | 12 | 12 |
|   | J | K | 14 | 14 |
| 2 | G | I | 15 + 12  | 27 |
|   |   | J | 15 + 14  | (28) |
|   | H | I | 12 + 13 | 25  |
|   |   | J | 14 + 12 | (26)  |
| 3 | D | G | 27 + 5 | 32 |
|   | E | G | 27 + 9 | 36 | 
|   |   | H | 25 + 12 | (37) |
|   | F | H | 25 + 13 | 38 |
| 4 | B | D | 32 + 4 | 36 |
|   |   | E | 36 + 4 | 40 |
|   | C | E | 36 + 9 | (45) |
|   |   | F | 38 + 6 | 44 |
| 5 | A | B | 36 + 8 | 44 |
|   |   | B | 30 + 8 | (48) |
|   | A | C | 44 + 4 | 48 |

To find the minimum we keep track of the total for each path.
The largest value from each set is removed. E.g. there is no reason to pass through G from J as it can be reach faster from I, and there is no reason to pass through E from H as it can be reached faster from G.

#### Maximum

To find the maximum from above we would eliminate the smaller values instead.

#### Maximin

| Stage | State | From | Value |
| --- | --- | --- | --- |
| 1 | H | K | 18 |
|   | I | K | 15 |
|   | J | K | 12 |
| 2 | E | H | (17) |
|   |   | I | 15 |
|   | F | H | (15) |
|   |   | I | 14 | 
|   |   | J | 12 |
|   | G | I | (14) |
|   |   | J | 12 |
| 3 | B | E | 11 |
|   |   | F | (13) |
|   | C | E | 12 | 
|   |   | F | 13 |
|   |   | G | (14) |
|   | D | F | (15) |
|   |   | G | 14 |
| 4 | A | B | 12 |
|   |   | C | (14) |
|   |   | D | 13 | 

ACGIK

In each case the highest value in getting to the new state is selected.
When tracing back we move from A to C (14), from C to G (14), G to I (14), and finally I to K (15).
The minimum value is 14.


#### Minimax

For each stage from each node, choose the minimum value.

| Stage | State | From | Value |
| --- | --- | --- | --- |
| 1 | H | K | 2.7 |
|   | I | K | 2.3 |
|   | J | K | 2.5 | 
| 2 | E | H | 2.7 |
|   |   | I | (2.4) |
|   | F | H | 2.7 |
|   |   | I | 2.6 |
|   |   | J | (2.5) |
|   | G | I | (2.6) |
|   |   | J | 2.9 |
| 3 | B | E | 2.8 |
|   |   | F | (2.7) | 
|   | C | E | 2.8 |
|   |   | F | (2.5) |
|   |   | G | 2.6 | 
|   | D | F | 2.8 |
|   |   | G | (2.6) |
| 4 | A | B | 2.7 |
|   |   | C | (2.5) | 
|   |   | D | 2.7 |

Now move backwards, picking the starred lowest values.

From A to C with 2.5, then C to F with 2.5, then F to J with 2.5, then J to K with 2.5.

ACFJK

### Negative edge lengths





## Network flows

**Source** The start of a network

**Sink** The end of a network

**Capacity** The weight on an arc

**Saturated** An arc is said to be saturated if the flow through it is equal to its capacity

**Cut** A continuous line which separates the source from the sink. A cut must not pass through any nodes

### Maximum flow minimum cut theorem

The maximum flow is less than or equal to the value of the minimum cut.

If a flow and cut with the same value can be found, the maximum flow has been found.


### Labelling procedure for flow augmentation

 1. Begin with any initial flow, or zero flow
 2. Replace each arc with two arcs, the forward arc showing excess capacity, and the backwards arc showing potential backflow (The current flow)
 3. If the source is still connected to the sink, find a new flow and alter the excess capacities and potential backflows as necessary
 4. Repeat step 3 until the source is disconnected from the sink. If the source is disconnected from the sink, then the maximum flow is the sum of all the flows of steps 1 and 3

The maximum flow can also be determined by tracing the flow.

### Super sources and sinks

Some problems may have multiple sources or sinks.
These problems can be solved by combining the sources and sinks into super sources and super sinks respectively.

In order to apply the labelling procedure, create the super source which feeds the existing sources with capacity sufficiently large so as not to affect the solution. Create a super sink in the same manner.

The maximum flow through the network can then be found by removing the super sources and sinks once the process is complete.

### Restricted capacity

**Restricted capacity** If a node has restricted capacity, there is a limit to the flow which can pass through it

In order to solve a problem involving restricted capacity, the restricted node must be represented by two unrestricted nodes, with an arc between of the same capacity as the restricted node.

If a node has multiple entrances, each of them must flow through the first unrestricted node.

### Minimum capacities

**Minimum capacity** An arc with a minimum is required to have a minimum flow through it

It is convention to label each directed arc with two numbers, an upper capacity and a lower capacity.

For such a network the value of a cut is defined as follows

> The value of a cut is the sum of the upper capacities for arcs that cross the cut line in the direction from the source to the sink minus the sum of the lower capacities for arcs that cross the cut line in the direction from the sink to the source


## Linear programming

### The Simplex method

The simplex algorithm simultaneously solves the equations of the constraints to find the nodes. It then works out whether that node maximises the objective function.

#### Slack variables

Slack variables are used in order to remove inequalities from each equation.

#### The tableau

The top row of the tableau is the objective function.
There is one row for each further equation.

Suppose we have $x + 2y \leq 6 \to x + 2y + s = 6$ and $x + y \leq 5 \to x + y + t = 5$ the tableau would be:

| $P$ | $x$ | $y$ | $s$ | $t$ | Value | Equation |
| --- | --- | --- | --- | --- | --- | --- |
| 1 | -2 | -1 | 0 | 0 | 0 | 1 |
| 0 | 1 | 2 | 1 | 0 | 6 | 2 |
| 0 | 1 | 1 | 0 | 1 | 5 | 3 |

#### Method

Once the tableau has been created, choose any column which has a negative number in the top row.
The aim is now to manipulate the equations to eliminate this variable from the other equations.

To do this a pivot value is chosen. The pivot is the value in the selected column which has not already been a pivot and provides the smallest value when equation value is divided by it.

In the case above the column chosen would be $x$, and the pivot value would be $1$ in equation 3 as $\frac 5 1 < \frac 6 2 < \frac 0 2$.

Divide the pivot row to get $1$ in the select column.
Then add a multiple of the new pivot equation to each of the other equations.

If the top row has no negative elements, stop.
If it still contains negative elements, find a new pivot from the new equations and repeat.

#### Reading the solutions

Find the maximum by interpreting the tableau as described below:
- $P$ equals the value in the top row
- Any non zero elements in the top row equal zero
- The other equations are then used to find the remaining variables

## Game theory for zero sum games

**Zero sum game** A game such that the gain for one party is exactly equal to the loss for the other party

### Pay off matrix

The pay off matrix represents the outcomes of each pair of decisions.
The outcomes are the pay offs to the player on the left side of the matrix, and as a result the losses for the player on the top of the matrix.

### Play safe strategies

A play safe strategy is to choose the option whose word outcome is as good as possible

Method
 - Add an extra column of the worst values from each row (The worst outcomes for the player on the left)
 - Add an extra row of the worst values from each column (The worst outcomes for the player on the top)

#### Stable solutions

If MAX(MIN(row)) = MIN(MAX(column)) there is a stable solution.

If a game has a stable solution, then both players should adopt a play safe strategy and the pay off from these strategies is called the value of the game.


### Dominance

A row of a pay off matrix can be ignored if each element is less than or equal to the corresponding element of another row.
A column of a pay off matrix can be ignored if each element is greater than or equal to the corresponding element of another column,

### $2 \times n$ games

For each column there is a line on the graph.
The optimal value is at the lowest intersection on the graph

To solve a $2 \times n$ game between A and B

 1. Assign probabilities of $p$ and $1 - p$ to the two options for A
 2. Plot A's expected pay off under each of B's options as lines on a graph of pay off against $p$
 3. Shade the region which is below every line
 4. Find the value of $p$ at the highest point of the shaded region
 5. Use this value of $p$ to find the value, $V$, of the game
 6. Assign probability $q$ to one of those options for B which determine the highest point of the shaded region
 7. Find the value of $q$ which gives B an expected pay off of $-V$ under either of A's options

### Finding the mixed strategy with simplex

If there are more than two rows, the mixed strategy can be found with simplex.

1. Let the probabilities of each of the options for A be $p_1, p_2, p_3, ...$
2. If necessary, add a constant value $k$ to eliminate any negative pay-offs from the matrix
3. The objective is then to maximise $V-k$ subject to the following constraints
	1. $V \leq \text{expected pay off from 1}$
	2. $V \leq \text{expected pay off from 2}$ ...
	3. $p_1 + p_2 + p_3 + .. \leq 1$ 
	4. $p_1, p_2, p_3, ... \geq 0$

