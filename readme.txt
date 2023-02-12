INTRO

Sudoku is a puzzle played on a 9x9 grid which is further separated into 9 smaller 3x3 boxes. The aim of the puzzle is to fill in every cell in the grid while following these rules: every cell on the grid must be filled and no number must appear within its local box, row or column more than once.


The amount of trial and error required to solve a sudoku means this puzzle is perfectly suited towards a backtracking approach seen in AI, commonly implemented through a search tree (implicit or explicit). Not only is Sudoku suited to these tactics, but it also has set rules (constraints) that every cell (variable) must follow within its row, column and box (domain). 

MY IMPLEMENTATION 

My agent utilises constraint propagation, constraint satisafction and a backtracking through a depth first search tree.

First, I defined the constraints of my solution, these would be the rules of Sudoku:
1.	Every cell must be filled with a number (1-9)
2.	Every cell in every row must hold a unique value (1-9)
3.	Every cell in every column must hold a unique value (1-9)
4.	Every cell in every box must hold a unique value (1-9)

By utilising numpy arrays and lists the agent could validate its moves when navigating the search tree through iterating over the row, column or box that the cell was located in.

Next, I defined my state and what would be contained within it. I would require a list of final values (final_values) which had already been placed, this had to be a 2D numpy array due to the specification, so it made sense to utilise the sudoku grid being passed into the sudoku_solver function as an argument to establish a starting partial state.

From there the agent builds up the possible values that could be placed into the Sudoku grid. This is done through a method build_possible_values, where if there is a none-zero value at a certain index of the partially filled sudoku array, then no possible values can be placed there. Otherwise, the function builds up a list of values 1-9. These initial values are then propagated removing any potential for multiple values to be placed in the same row/column/box, hence maintaining constraint satisfaction. By giving the agent all of the possible moves for a particular cell it greatly increases the efficiency when checking whether a particular move is valid, and whether a backtrack is required. This is because the variables can be checked for constraint 1, as no variable should ever be at a 0 value, while also having no potential moves.

From the initial_propogration method the sudoku grid is also checked for solvability, this allows the agent to determine whether values already exist in the same domain and therefore whether any of the constraints have been broken or not. Hence, increasing the efficiency of the sudoku solver by eliminating the chance of solving an unsolvable sudoku puzzle.

This Sudoku state is then solved used a Depth First search (adapted from the eightqueensrevisited notebook), starting at the most constrained variable after the initial_propogration. At the cost of some minor variable changes the most constrained variable approach proves to be a very effective way at reducing the search space, through restricting and propagating the potential guesses of other variables who share the same domain (constraint 2,3,4). 

A depth first search is not an optimal algorithm however, when solving a sudoku the most optimal route is not a factor, all we want is a valid solution to the sudoku grid. Hence, it is well suited to this problem due to the way it builds upon the most recently discovered state and allows us to reach a near end goal of completing the sudoku relatively quickly. This is good as Sudoku's can have multiple different solutions and by delving deep into one specific branch of the search tree, we discover potential solutions a lot quicker than if we utilised a breadth first search and explored all the possible ways of solving the puzzle.

LIMITATIONS AND FUTURE DEVELOPMENT

One limitation I encountered while testing my initial runtime of my solution was the impact that the “copy.deepcopy()” method has on the efficiency of the agent. I overcame this impact through creating my own methods which create copies of the list objects stored within my states.

One alternative method that I discovered while researching how to create an efficient agent to solve sudoku puzzles, was Algorithm X. This algorithm designed by Donald Knuth to find every solution to exact cover problems. Sudoku is a textbook example of an Exact Cover problem due to its constraints and possible assignments to each cell. Algorithm X can be realised through an agent using the Dancing Links algorithm; however, this algorithm requires building multi-linked node lists and is hard to implement but is the most efficient general algorithm for solving these exact cover problems. However due to the large complexity of the algorithm I was unable to produce a working implementation. If I were to create an agent for a future exact cover problem, I would utilise the Dancing Links algorithm to do so.


