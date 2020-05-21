---
layout: post
title: Multi-Agent path finding(MAPF) & Search algorithm
category: Multi-Agent Path Finding
tag: Multi-Agent Path Finding
---

# Multi-Agent path finding(MAPF)
<a href="https://postimg.cc/QF5ChNVj"><img src="https://i.postimg.cc/mgpH3ts1/image.png" width="700px" title="source: imgur.com" /></a>
- Multi-Agent(many robot) play to find the target position and move
- MAPF problem: Find a collision-free plan(path) for each agent
- Another name we called it : Cooperative path finding(CPF), Multi-robot path planning, pebble(鹅卵石) motion

## 1) Introduction to MAPF
<a href="https://postimg.cc/ZWRRvPfK"><img src="https://i.postimg.cc/bw02M3kb/51212413.png" width="700px" title="source: imgur.com" /></a>
- a graph (directed or undirected)
- a set of agents(robot), each agent is assigned to two locations(nodes) in the graph(start, destination)
- Each agent can perform either move(to a neighbouring node) or wait(in the same node) actions

> Typical Assumption:
 all move and wait actions have identical durations(plans for agents are synchronized)

- **Plan** is a sequence of actions for the agent leading from its start location to its destination
  - the **length of plan**(for an agent) is defined by the time when the agent reaches its destination and does not leave it anymore.
- Find **Plans** for all agents such that the plans **do not collide in time and space** (no two agents are at the same location at the same time)
<a href="https://postimg.cc/4mscxFjD"><img src="https://i.postimg.cc/Pr8QKgy5/50301230.png" width="500px" title="source: imgur.com" /></a>
- some necessary **conditions for plan existence**:
  - no two agents are at the same start node
  - no two agents share the same destination node(unless an agent disappears when reaching its destination)
  - the number of agents is strictly smaller than the number of nodes
- agent at $v_i$ cannot perform move $v_j$ at the same time when agent at $v_j$ perform move $v_i$
- Agents may swap position. but agents use the same edges at the same time, swap is not allowed
<a href="https://postimg.cc/qgyk5JDr"><img src="https://i.postimg.cc/FFn711jY/512124123.png" width="500px" title="source: imgur.com" /></a>
- Agent can approach node that is currently occupied but will be free before arrival(Agent at $v_i$ cannot perform move $v_j$ if there is another agent at $v_j$)
<a href="https://postimg.cc/dDxxrrjt"><img src="https://i.postimg.cc/ZKbzkL93/5123124123.png" width="500px" title="source: imgur.com" /></a>
- if any agent is delayed then trains may cause collision during execution
- to prevent such collisions we may introduce more space between agents

### K-robustness
<a href="https://postimg.cc/3WNK3jwK"><img src="https://i.postimg.cc/J7ctcPVB/512512.png" width="500px" title="source: imgur.com" /></a>
- An agent can visit a node, if that node has not been occupied in recent *k* steps.
- 1-robustness covers both no-swap and no-train constraints (1번밖에 못 거치는것)

### Other constraints
- No Plan(path) has a cycle
- No Two plans(paths) visit the same location
- waiting is not allowed
- some specific locations must be visited

### How to Measure Quality of plans?
There are two typical criteria(to minimize):
<a href="https://postimg.cc/QKBGcr4f"><img src="https://i.postimg.cc/8c4pQphg/05050.png" width="500px" title="source: imgur.com" /></a>
- **Makespan**
  - distance between the start time of the first agent and the complete time of the last agent(아무개 에이전트의 시작과 아무개 에이전트의 마지막 동작)
  - maximum of lengths of plans(end times)
- **Sum of costs(SOC)**
  - Sum of lengths of plans(end times)
- Optimal single agent path finding is tractable(易处理的)
  - e.g. Dijkstra's algorithm
- Sub-optimal Multi-Agent path finding(with two free unoccupied nodes) is tractable
  - e.g algorithm Push and Rotate
- MARF, where agents have joint goal nodes(it does not matter which agent reaches which goal) is tractable
  - reduction to min-cost flow problem
- Optimal(Makespan,SOC) multi-agent path finding is **NP-HARD**
<a href="https://postimg.cc/HrtYYdfM"><img src="https://i.postimg.cc/jSpLhjcM/1511314.png" width="700px" title="source: imgur.com" /></a>

### Solving approaches
{% include advertisements_horizon.html %}
1. **Search-Based Techniques**
- state-space search(A*)
  - state = location of agents at nodes
  - transition = performing one action for each agent
- Conflict-based search
2. ** Reduction-based Techniques
- Translate the problem to another formalism(形式主义)
  - SAT/CSP/ASP ...

## 2. Search-Based Solvers
- Search is a General Problem solving technique
- To expand or not expand, this is the question
<a href="https://postimg.cc/PN88tvK8"><img src="https://i.postimg.cc/h42819dM/511313.png" width="700px" title="source: imgur.com" /></a>
- Why do we need **SEARCH** for MAPF? Because it is Finding an optimal solution to hundreds of agents
- this is classical application of Search
<a href="https://postimg.cc/gxf3yyMd"><img src="https://i.postimg.cc/5ttpVnLH/image.png" width="700px" title="source: imgur.com" /></a>
- Solving Multi-Agent Path Finding with Search
<a href="https://postimg.cc/Q96tmhX5"><img src="https://i.postimg.cc/RhmHMZyP/5123124213.png" width="500px" title="source: imgur.com" /></a>
- From A* to **prioritized planners**
- From prioritized(按重要性排列) planners back to A*
- The **Increasing Cost tree search**(ICTS)
- The **Conflict-Based Search**(CBS) framework
- Approximately optimal search-based solvers
<a href="https://postimg.cc/XrVYRrpF"><img src="https://i.postimg.cc/mg91FM4d/51241241.png" width="500px" title="source: imgur.com" /></a>
- Single Agent Search Problem Properties
  - Number of state = ? (it will be 4 )
  - Branching factor = ? (4)
<a href="https://postimg.cc/JD1nLtTS"><img src="https://i.postimg.cc/nzmQqDqV/444-12.png" width="500px" title="source: imgur.com" /></a>
- Two Agent Search Problem Properties
  - Number of States = ? ($20^2$)
  - Branching factor = ? ($5^2$)
- then, What about **K** Agents?
  - Number of States = ($20^k$)
  - Branching factor = ($5^k$)
- **IT IS VERY HARD PROBLEM**
- SO Key Idea: Plan for each agent seperately
- Challenge: Maintaining soundness, completeness, and optimality
<a href="https://postimg.cc/xkH24Ynh"><img src="https://i.postimg.cc/MG3pXK46/5030123.png" width="500px" title="source: imgur.com" /></a>

### Prioritized Planning (Silver 2005) Analysis: First Agent
<a href="https://postimg.cc/9rqJTfp6"><img src="https://i.postimg.cc/Kv0V6kg8/513121.png" width="500px" title="source: imgur.com" /></a>
### Prioritized Planning (Silver 2005) Analysis: Second Agent
<a href="https://postimg.cc/R3cFy2Bs"><img src="https://i.postimg.cc/0N4wMqHs/51212313.png" width="500px" title="source: imgur.com" /></a>

- Complexity?
  - Polynomial in the grid size and max time
- Soundness?
  - Yes
- Complete? Optimal?
  - No
{% include advertisements_horizon.html %}
- Smart Agent **Prioritization**
  - conflict oriented WHCA*[Bnaya and Felner ‘14]
  - Re-prioritization and safe intervals [Andreychuk and Yakovlev ‘18]
- Integrate **planning and execution**
  - Windowed Hierarchical CA* [Silver ‘06]
<a href="https://postimg.cc/mPSZSCNJ"><img src="https://i.postimg.cc/cLpK0cKx/512314123213.png" width="700px" title="source: imgur.com" /></a>
- High-level idea : reservation-based planning (ex) Number of states = 4 x 5 x maxTime)
- **FAST**, requires almost no coordination
  - but **incomplete** and not **optimal**
### Search based solver overview
<a href="https://postimg.cc/LgB40PFJ"><img src="https://i.postimg.cc/7hjCh1Bn/51232131231.png" width="500px" title="source: imgur.com" /></a>
- Can MARF algorithm be **Complete** and **efficient**?

### MAPF as a puzzle
<a href="https://postimg.cc/7G4SCg96"><img src="https://i.postimg.cc/B6LpVp1x/5112313123.png" width="500px" title="source: imgur.com" /></a>
- MAPF is highly related to **pebble motion problems**
  - Each agent is a pebble(조약돌)
  - Need to move each pebble to its goal
  - cannot put two pebbles in one hole
- **Pebble Motion can be solved Polynomial**
  - but far from optimally
  - complex formulation
- Similar approaches
  - slidable(滑动) Multi-Agent Path Planning[Wang & Botea, IJCAI, 2009]
  - Push and Swap [Luna & Bekris, IJCAI, 2011]
    - Parallel push and swap [Sajid, Luna, and Bekris, SoCS 2012]
    - Push and Rotate [de Wilde et al. AAMAS 2013]
  - Tree-based agent swapping strategy [Khorshid at el. SOCS, 2011]

### Examples
 <a href="https://postimg.cc/7G4SCg96"><img src="https://i.postimg.cc/B6LpVp1x/5112313123.png" width="500px" title="source: imgur.com" /></a>

### Search based solver Summary
<a href="https://postimg.cc/PP9Jr9m4"><img src="https://i.postimg.cc/3xJv9HNP/5123124.png" width="500px" title="source: imgur.com" /></a>

### Can a MAPF algorithm be complete and efficient and optimal?
<a href="https://postimg.cc/tYsr0nqc"><img src="https://i.postimg.cc/Nf4ZHRWQ/155123.png" width="500px" title="source: imgur.com" /></a>
*On the Complexity of Optimal Parallel Cooperative Path-Finding, Surynek 2015*
*Planning Optimal Paths for Multiple Robots on Graphs, Yu and LaValle, 2013*

### From Tiles to Agents
<a href="https://postimg.cc/s1qLg4Nz"><img src="https://i.postimg.cc/c4LGTD7v/125513.png" width="500px" title="source: imgur.com" /></a>
- Can we adpat technique from these extreme cases? answer is YES(invent some new techniques also(optimal MAPF))
<a href="https://postimg.cc/WDgnz6N4"><img src="https://i.postimg.cc/bvFF6341/41232213123123.png" width="500px" title="source: imgur.com" /></a>

1. Searching the k-agent search space
- A*+OD+ID [Standley ‘10]
- EPEA* [Felner ‘X, Goldenberg ‘Y]
- M* [Wagner & Choset ‘Z]
2. Other search-based approaches
- ICTS [Sharon et al ‘13]
- CBS [Sharon et al ’15]

### Optimal MARF with A*
<a href="https://postimg.cc/qhDQ5yFj"><img src="https://i.postimg.cc/mkBqhSyf/512312312.png" width="500px" title="source: imgur.com" /></a>
- A* expands nodes
- A* gain efficiency by choosing which node to expand
> What is the complexity of expanding **a single node** in MARF with 20 agents?

> $5^20$ = 95,367,431,640,625

### Search Tree Growth with **Operator Decomposition**
<a href="https://postimg.cc/gnhF32wf"><img src="https://i.postimg.cc/ncRFbsnc/51231313.png" width="500px" title="source: imgur.com" /></a>
- Pros
  - Branching factor is reduced to 5(= single agent)
  - with a perfect heuristic can solve the problem
- Cons
  - Solution is deeper by a factor of k
  - More nodes may be expanded, due to intermediates

### Enhanced Partial Expansion A*
<a href="https://postimg.cc/mhVbpDVJ"><img src="https://i.postimg.cc/151ftVFm/55512312.png" width="500px" title="source: imgur.com" /></a>

### Independence Detection
<a href="https://postimg.cc/grXtwtQJ"><img src="https://i.postimg.cc/pVkw6gBD/512314213.png" width="500px" title="source: imgur.com" /></a>
- Simple Independence Detection
  - Solve optimally each agent separately
  - while some agents conflict
    - 1. Merge conflicting agents to one group
    - 2. solve optimally new group
<a href="https://postimg.cc/4n1vxrQV"><img src="https://i.postimg.cc/v8J03y1q/4123124123.png" width="500px" title="source: imgur.com" /></a>
- Independence Detection
  - Solve optimally each agent separately
  - While some agents conflict
    - 1. Try to avoid conflict, with the same cost
    - 2. Merge conflicting agents to one group
    - 3. Solve optimally new group
- but what if we have an environment like that
<a href="https://postimg.cc/sv9Ly6vN"><img src="https://i.postimg.cc/Fs2XywmN/55123.png" width="200px" title="source: imgur.com" /></a>

### M* (Wagner & Choset ‘11,’14)
<a href="https://postimg.cc/0KpZZQTN"><img src="https://i.postimg.cc/ZnQgbvj6/44.png" width="500px" title="source: imgur.com" /></a>
- Find optimal path for each agent individually
<a href="https://postimg.cc/nXB1KCt9"><img src="https://i.postimg.cc/cL55Sn9h/2121.png" width="200px" title="source: imgur.com" /></a>
- Start the search. Generate only nodes on optimal paths
<a href="https://postimg.cc/kVxtGptk"><img src="https://i.postimg.cc/1zd0CxR5/51333.png" width="400px" title="source: imgur.com" /></a>
- If conflict occurs – backtrack and consider all ignored actions

### **Recursive M*** (Wagner & Choset ‘11,’14)
<a href="https://postimg.cc/SXBFTgTN"><img src="https://i.postimg.cc/633WQSCR/5123123124.png" width="500px" title="source: imgur.com" /></a>
- Recursive M*
  -  Find optimal path for each agent individually
  -  Start the search. Generate only nodes on optimal paths
  -  If conflict occurs – backtrack and consider all ignored actions
     - Apply M* recursively after backtracking
- but problem is :
<a href="https://postimg.cc/TKQnJSPg"><img src="https://i.postimg.cc/MGJtqJ2P/5532.png" width="500px" title="source: imgur.com" /></a>
- joint path up to bottleneck can be long

### Other Search-based approaches(ICTS,CBS)
1. **Increasing Cost Tree Search** (Sharon et al. ‘12)
<a href="https://postimg.cc/hhJSc3Qs"><img src="https://i.postimg.cc/SQdjLByB/5213.png" width="500px" title="source: imgur.com" /></a>
<a href="https://postimg.cc/T5rbS2rN"><img src="https://i.postimg.cc/J0FQsGt8/553.png" width="500px" title="source: imgur.com" /></a>
<a href="https://postimg.cc/vDJFtY2F"><img src="https://i.postimg.cc/xTCfYqKC/33123.png" width="500px" title="source: imgur.com" /></a>
<a href="https://postimg.cc/bdYtBGCW"><img src="https://i.postimg.cc/MZVmXVy6/12343.png" width="500px" title="source: imgur.com" /></a>
- Does it work? - Not always
<a href="https://postimg.cc/gn8mysyW"><img src="https://i.postimg.cc/BQwb0wXX/414123123.png" width="500px" title="source: imgur.com" /></a>

2. Conflict Based Search(CBS)
- CBS only performs single agents(But, many times, and under different constraints)
<a href="https://postimg.cc/2VMTYdgV"><img src="https://i.postimg.cc/mg2nQ8wN/4123123.png" width="500px" title="source: imgur.com" /></a>
- Conflict: agent 1 and agent 2, plan to occupy C at time 2
- Constrain: agent 1, avoid C at time 2 or agent 2 to avoid C at time 2
- The Constraint Tree
  - Nodes:
    - A set of individual constraints for each agent
    - a set of paths consistent with the constraints
  - Goal Test:
    - Are the paths conflict free
<a href="https://postimg.cc/4nNTHZyp"><img src="https://i.postimg.cc/7PSPcZyt/412413.png" width="500px" title="source: imgur.com" /></a>

- Analysis: *Example 1**
  - How many states A* will expand?
  - How many states CBS will?
  - Motivation: case with bottlenecks:
  - A*
    - g+h=6: All $m^2$ combinations of ($A_i$,$B_j$) will be expanded
    - f=7 : 3 states are expanded
<a href="https://postimg.cc/WhNKf7gr"><img src="https://i.postimg.cc/m2CskXrd/333.png" width="500px" title="source: imgur.com" /></a>
    - A* $m^2$ = O($m^2$)
    - CBS : 2m+14 = O(m) states
  - **When m > 4 CBS will examine fewer states than A***
- Analysis: *Example 2**
  - States expanded by CBS?
  - States expanded by A*?
<a href="https://postimg.cc/XB8PYZSp"><img src="https://i.postimg.cc/xqDVgH6y/44321.png" width="500px" title="source: imgur.com" /></a>
  - 4 optimal solutions for each agent
  - Each pair of solutions has a conflict
  - Rough analysis:
    - CBS: exponential in #conflicts = 54 states
    - A*: exponential in #agents = 8 states
- **Trends observed**
  - In open spaces: use A*
  - In bottlenecks: use CBS
- *What if we have both?*

### Meta-Agent CBS (MA-CBS)
- Plan for each agent individually
- Validate(确证) plans
- If the plans of agents A and B conflict
```
If (should merge(A,B))
  merge A and B into a meta-agent
  and solve with A*
Else
```
- Constrain A to avoid the conflicts or Constrain B to avoid the conflict
- Should Merge(A,B) (simple rule):
  - Merge when observed more than T conflicts between A,B
<a href="https://postimg.cc/QKmWYD52"><img src="https://i.postimg.cc/Bt9c6Qvn/421313.png" width="500px" title="source: imgur.com" /></a>
<a href="https://postimg.cc/zVRyRTPm"><img src="https://i.postimg.cc/wTwLZVXB/1234321.png" width="500px" title="source: imgur.com" /></a>

### CBS Enhancements
- Which conflict to resolve? [Boyarski et al. ‘16]
- What to do after merging? [Boyarski et al. ‘16]
- Heuristics for the constraint tree search [Felner et al. ‘18]
- Augmenting CBS with human knowledge [Cohen et al.]
- Which low-level solver to use?
- When to merge the agents ?

### Summary
- A* (M*, EPEA*, A*+OD+ID)
  - Main factors: #agents, graph size, heuristic accuracy
- ICTS
  - Main factors: #agents, Δ, graph size
- CBS and its variants
  - Main factors: #conflicts
<a href="https://postimg.cc/Yhg1wc7m"><img src="https://i.postimg.cc/L6QvLR0v/4441233.png" width="500px" title="source: imgur.com" /></a>
<a href="https://postimg.cc/zL3v8WhP"><img src="https://i.postimg.cc/nLGDKGhZ/43213.png" width="500px" title="source: imgur.com" /></a>

### Solving MAPF
<a href="https://postimg.cc/PPhWbt0r"><img src="https://i.postimg.cc/L6gxp5Tf/43123.png" width="500px" title="source: imgur.com" /></a>
### Bounded Suboptimal Algorithms
- An algorithm is bounded suboptimal iff
  - It accepts a parameter !
  - It outputs a solution whose cost is at most 1 + ! ⋅Optimal
- How to create a bounded suboptimal algorithm?
  - Different search algorithms
  - Inadmissible(不允许的) heuristics

### Suboptimal ICTS
<a href="https://postimg.cc/rzLJFCVH"><img src="https://i.postimg.cc/jqDkcZGR/41233.png" width="500px" title="source: imgur.com" /></a>
### Suboptimal A*
<a href="https://postimg.cc/R6GNmBV4"><img src="https://i.postimg.cc/d3cd8sbk/43211.png" width="500px" title="source: imgur.com" /></a>
### Suboptimal M*
<a href="https://postimg.cc/t7w9xTc8"><img src="https://i.postimg.cc/7YYhjJnP/22211.png" width="500px" title="source: imgur.com" /></a>
### Suboptimal CBS
<a href="https://postimg.cc/pm1C2wrQ"><img src="https://i.postimg.cc/jjRFhKr1/42133.png" width="500px" title="source: imgur.com" /></a>
- Observation: Suboptimality can be introduced in both levels
  - ECBS (Barer et al. ‘14)
  - ECBS+Highways (Cohen et al. ’15, ‘16)
<a href="https://postimg.cc/3yfxfZLx"><img src="https://i.postimg.cc/vBbxBqff/4312321.png" width="500px" title="source: imgur.com" /></a>

### Advanced Issues in Search-based MAPF Algorithms
- When to use which algorithm? Ensembles(全体)?
- Using knowledge about past plans (Cohen et al. ’15)
- Stronger heuristics for all algorithms
- Deeper analysis of algorithms’ complexity
- Beyond grid worlds
  - Kinematic constraints (Ma et al. ‘16)
  - Any-angle planning (Yakovlev et al. ‘17)
  - Hierarchical environments (Walker et al. ’17)
  - Large agents (Li et al. ‘19)
- Priorized planning based on CBS (Ma et al. ‘19)
- Planning & execution


Reference: [postassco](https://potassco.org/doc/start/)
