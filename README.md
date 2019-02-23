# Non-deterministic Turing Machine Simulator

A C program that simulates a Non deterministic Turing Machine programmed to recognize a specific language. 
It uses a formatted input file, containing, in order:
- a list of transitions (```tr```), that specify the behaviour of the head, e.g.
  ```
  tr
  0 a c R 1
  ...
  ```
  In the example above, 
  - ```0```: the starting state of the transition
  - ```a```: the char read by the head on the pointed cell of the tape
  - ```c```: the char that the head will write on the pointed cell of the tape
  - ```R```: the direction in which the tape has to move after writing (L is left, R is right)
  - ```1```: the final state of the transition
- an acceptance state (```acc```)
- a maximum number of valid transitions before stopping, to avoid potential infinite loops (```max```)
- the strings that need to be recognized by the machine (```run```)

After running all the strings, it will write the outcome of every verification on a output file (one line per string):
- ```1``` if the machine has stopped into an acceptance state
- ```0``` if the machine has stopped before reaching an acceptance state and never exceeding the max number of moves (after exploring every possible branch)
- ```U``` if the machine has stopped before reaching an acceptance state AND exceeded the max number of moves at least once

### Logic

A non-deterministic Turing Machine differs from the deterministic model in the order of execution of the transitions: since a state can have two or more transitions where the read character is the same and the outcome is different (different written character and different move on the tape), there is no specific path of execution, rather a computation tree that contains every single possible branch of every outcome. 
The problem is that building a tree that continously allocate memory by saving the tape every time so it can track every move and every "state" of the machine at that moment, would increase the space and time complexities **by a wide margin**. 

So I've opted to build a **stack** containing one possible stream of branches, and whenever a non acceptance state (or the max number of moves) is reached, the head is reversed, (so we're popping the transitions from the stack and the tape is reverted to the previous state) and the other possible branches are tested. No tape is saved in the stack, only the transitions.
So we're reverting all the moves by going up the computation tree until there are unexplored branch on the same level, and pursuing those. When the head cannot move anymore (all the branches were tested), and we've exceeded at least once the maximum number of moves, then we've reached the ```U``` output, which means that
- no acceptance state was reached
- the max number of moves was exceeded (at least once)

The tape is considered infinite in both directions, so there are blank/empty cells in front and at the end of the currently placed string. 

### Usage

- Compile the C program with your compiler of choice. 
- Place your txt files containing the machine configuration (e.g. ww.txt or wwR.txt) inside the exe folder. Remember to change the path of the checked files in the read function, inside ```main()```.
- Run the program.

### Why?

The program was the final assignment of an Information Theory course of IT Engineering at Politecnico di Milano.
