One of many ways to formally [[Process Modelling|model a process]].

Consists of:
- A finite set of states $Q$
- An alphabet $\Sigma$
- A transition function $\delta$ such that $\delta: Q \times \Sigma \rightarrow Q$
- A starting state $q_0 \in Q$
- A set of accepting states $F \subseteq Q$

![[Pasted image 20250106091552.png]]
*Figure 1: The process of passing the exam in Reactive and Event-Based Systems.*

In figure 1, we have
$Q = \{Q_0, Q_1, Q_2, Q_3, Q_4\}$,
$\Sigma = \{\text{Pass Assignments (PA)}, \text{Pass Exam (PE)}, \text{Study (S)}\}$

The accepting states in this finite state automata are $Q_2$ and $Q_4$ and are denoted with double circles.

### Runs/Executions
The different ways we can execute the process, consisting of the intermediate states and steps taken.

The runs of the process in figure 1 are
```
Q0
Q0 -PA-> Q1
Q0 -PA-> Q1 -PE-> Q2
Q0 -PA-> Q1 -S->  Q3
Q0 -PA-> Q1 -PE-> Q2 -S->  Q4
Q0 -PA-> Q1 -S->  Q3 -PE-> Q4
```

### Accepting Runs/Executions
The different ways we can execute the process, consisting of the intermediate states and steps taken, which satisfy the accepting condition.

Accepting runs of the model in figure 1:
```
Q0 -PA-> Q1 -PE-> Q2
Q0 -PA-> Q1 -PE-> Q2 -S->  Q4
Q0 -PA-> Q1 -S->  Q3 -PE-> Q4
```

### Traces
The possible sequences of activities that the process can exhibit.

```
Runs/executions:                      Traces:
Q0                                    <>
Q0 -PA-> Q1                           <PA>
Q0 -PA-> Q1 -PE-> Q2                  <PA, PE>
Q0 -PA-> Q1 -S->  Q3                  <PA, S>
Q0 -PA-> Q1 -PE-> Q2 -S->  Q4         <PA, PE, S>
Q0 -PA-> Q1 -S->  Q3 -PE-> Q4         <PA, S, PE>
```

### Accepting Traces
The possible sequences of activities that the process can exhibit which correspond to an accepting run of the process.

```
Runs/executions:                      Traces:
Q0 -PA-> Q1 -PE-> Q2                  <PA, PE>
Q0 -PA-> Q1 -PE-> Q2 -S->  Q4         <PA, PE, S>
Q0 -PA-> Q1 -S->  Q3 -PE-> Q4         <PA, S, PE>
```

### Language
The set of all accepting traces.

```
Runs/executions:                      Traces:        Language:
Q0 -PA-> Q1 -PE-> Q2                  <PA, PE>       {<PA, PE>,
Q0 -PA-> Q1 -PE-> Q2 -S->  Q4         <PA, PE, S>     <PA, PE, S>,
Q0 -PA-> Q1 -S->  Q3 -PE-> Q4         <PA, S, PE>     <PA, S, PE>}
```

### Deadlock
A state in a system or process from which we can do no further actions, but which is not accepting.

![[Pasted image 20250106093355.png]]
*Figure 2: A model of a process in a deadlock.*

### Livelock
A state or set of states in a system or process, in which we can still do actions, but from which no accepting run is possible.

With one basic change, we can change the model in figure 2 to a model with a livelock:

![[Pasted image 20250106093634.png]]
