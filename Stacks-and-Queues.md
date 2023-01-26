## Introduction 

- Stacks and queues are higher level data structures 

- Can be implemented by an array, or something else...

- Motivation 

    - Model of program execution: 

        - main() routine and its variables 
        - `main()` routine calls function `f()` 
        - environment set up for `f()` calls its local variables 

            - we don't have access to variables in `main()` in `f()` 

        - `f()` calls `g()`, then `g()` calls `h()`

        - each time we suspend our current operation and goes into the called function, as the function returns, we resume previous function 

        - `h()` returns, `g()` left, then `g()` returns, `f()` left, then `f()` calls `g()`, then `g()` returns...

        - New environments get added, one gets removed, another gets added -> always at the end 
    
    - Some game program: 

        - Two players red and black 

        - Red figure out best move to make in the board position for biggest win -> write a program to consider all possible moves and check which one is best 

        - Make 1 move -> result in a changed board position 

            - check if that is a good move or not 

                - if end -> bad 
                - if win -> good 
                - if neither -> take the role of black 

        - Black will make another move -> change board position 

            - check again 
        
        - Red takes turn again

        - Each time we are considering a move, if that turns out bad, we move back and try adding another one 

            - operation of adding and removal to a sequence from the end 

        - same pattern of behavior of function calls 

---

## Stack 

- Sequence of items: 
    
    - The only place to insert an item is from one end

    - The only place to remove an item is from the same end 

- Why? 

    - I could just use an array or list for this --> but to an user an array/list's function suggests that you might use it to do some operations elsewhere

    - Using a stack tells the user how to use the data structure -> limits the possiblity -> easier reading 

### Stack operations 
