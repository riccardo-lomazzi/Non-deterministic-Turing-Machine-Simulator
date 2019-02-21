# Non-deterministic Turing Machine Simulator

A C program that simulates a Non deterministic Turing Machine programmed to recognize a specific language. 
It uses a formatted input file, containing, in order:
- a list of transitions (```tr```), that specify the moves of the pin head, e.g.
  ```
  tr
  0 a c R 1
  ...
  ```
  In the example above, 
  - ```0``` is the starting state of the transition
  - ```a``` : the char read by the pin head on that position of the tape
  - ```c```: the char to write on the position of the pin head
  - ```R```: the direction in which the tape has to move (L is left, R is right)
  - ```1```: is the final state of the transition
- an acceptance state (```acc```)
- a max number of moves before stopping (```max```)
- the strings that need to be recognized by the machine (```run```)

After running all the strings, it will write the output of every string on a output file (one line per string checked):
- ```1``` if the machine has stopped into an acceptance state
- ```0``` if the machine has stopped before reaching an acceptance state but also without exceeding the max number of moves
- ```U``` if the machine has stopped before reaching an acceptance state AND exceeded the max number of moves

### Logic

A non-deterministic Turing Machine differs from the deterministic model in the branching paths: since a state can have two or more transitions where the read character is the same and the output is different, there is no specific path, rather a computation tree that containers every single possible branch of every outcome. 
But building a tree that continously allocate memory by saving the tape every time, would increase the time complexity **a lot**. 
So I've opted to build a **stack** containing one possible stream of branches, and whenever a non acceptance state (or the max number of moves) is reached, the pin head is reversed, and the other possible branches are tested. 
So we're reverting all the moves by going up the computation tree until there are unexplored branch on the same level, and pursuing those. When the pin cannot move anymore (all the branches were tested), then we've reached the ```U``` output, which means that
- no acceptance state was reached
- the max number of moves was exceeded (at least once)

The tape is considered infinite in both directions, so there are blank/empty cells in front and at the end of the currently placed string. 

### Usage

- Compile the c program with your compiler of choice. 
- Place your txt files containing the machine configuration (e.g. ww.txt or wwR.txt) inside the exe folder
- Run it!

### Why?

The program was the closing assignment of an Information Theory course at IT Engineering of Politecnico di Milano.
