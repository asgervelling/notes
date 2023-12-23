The output of our computation is dependent on the arbitrary ordering of some operation to get to that output.

One thread increments `x`. Another reads it. Whoops.

Solve it with [[Mutex|mutexes]].


### Progress Graph
![[Pasted image 20231120132436.png]]
*Arbitrarily named states*.

##### Trajectories in Progress Graphs
![[Pasted image 20231120132626.png]]
*Note that you can only ever go right by one, or up by one. That's because these are [[Logical Control Flow|control flows]].*
