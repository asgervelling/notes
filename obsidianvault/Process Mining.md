In [[Reactive and Event Based Systems (REBS) | Reactive and Event Based Systems]], process mining means looking at real-world event logs to find out how processes actually work.

## Event Log
An event log is a list or table containing events consisting of
- Activities (e.g. consultation, treatment)
- Cases (e.g. a treatment or patient ID)
- An ordering of the events (e.g. timestamps)
- Optionally, any other data relevant to the process

| Activity (event type) | Patient | Time             | ... |
| --------------------- | ------- | ---------------- | --- |
| Consultation          | Bob     | 2024-01-02 10:30 |     |
| Consultation          | Alice   | 2024-01-04 09:00 |     |
| Treatment             | Bob     | 2024-01-06 14:00 |     |
Recently, there has been a movement towards object-centric event logs, which allow events to be tied to several objects, instead of just a single process instance, thereby better capturing the interactions between different objects, such as different users, stakeholders and physical resources.

The table above looks like a database format. We may also express events as traces, where each trace corresponds to a process instance.
- Bob: `<Consultation, Treatment>`
- Alice: `<Consultation>`

*Note*
Events in event logs are not the same as events in [[Dynamic Condition Response Graphs | DCR-graphs]]. In DCR-graphs, events may occur more than once. However, in event logs, each event is unique.

## Process Mining Visualization

Here is one way to visualize event logs.

![[Pasted image 20241219112348.png]]

In the visualization, we see that 28.79% of the behavioral patterns were the trace `<497, 469, 518, 539>`.

We can also visualize the log as a graph - as a transition system. Therefore, we need to express the transition system, as this is not captured in the event log. As there are many different allowed activities from a particular execution, such graphs can grow quite unwieldy.

![[Screenshot from 2024-12-19 11-29-46.png]]

Zooming in on the graph, we can see more detail.
The circles and boxes represent states, with the boxes being accepting states to the log. They are annotated by the number of times they were visited. Note that there is a bug, since the initial state shows only 1 visit, instead of the expected 594.
The edges represent event types, showing which activities were executed to move from one state to the next, and how often this occurred in the log.
This method of visualization is a great choice when you want to analyze traces, without having to look at all the possible traces, which would be counter-intuitive and difficult.
![[Pasted image 20241219113832.png]]


Instead of a transition system, we can also discover other types of models from the log.
Here we see a Petri net mine from the same log. The model is simpler, requiring fewer visual elements, but is also less precise. Nearly all activities can be skipped by firing the black silent transitions.

![[Pasted image 20241219114201.png]]

## Process Mining Algorithms
Process mining algorithms can be divided into three categories:
- Conformance checking
  - Check if real processes conform to our models
- Process discovery
  - Create models based on real-world behavior
- Process enhancement
  - Improve our models based on observed real-world behavior


