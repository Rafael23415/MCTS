---
title: "Monte Carlo Tree Search"
subtitle: "How MCTS works and how it compares to other search methods"
author: "Diogo Santos - 2241031 | Rafael Prior - 2240993"
format:
  revealjs:
    theme: black
    slide-number: true
    transition: slide
    center: false
    width: 1280
    height: 720
---

# Monte Carlo Tree Search (MCTS)

---

## What is MCTS?

- Monte Carlo Tree Search (MCTS) is a method used for problems with very large decision spaces.
- It uses random choices to build a tree from the results.
- The tree grows unevenly, using a formula to balance exploration and exploitation.
- It focuses most simulations on promising moves.

---

## MCTS vs. Other Methods

### Minimax
- Searches every move down to a fixed depth.
- Explores all possible states within that depth.

### Reinforcement Learning
- Both balance exploration and exploitation.
- Both estimate the value of actions using sampled outcomes.

### Monte Carlo
- Uses random simulations and aggregates their results.

---

## The Main Challenge of MCTS

- Perform as few playouts as possible.
- Search as deep as possible into promising moves.
- Some implementations use heuristics to estimate how certain paths will end without simulating them.

The objective is to obtain the best possible result while minimizing computational resources.

---

## The Four Stages of MCTS

MCTS has four stages inside a cycle:

1. **Selection** – Choose a node to explore.
2. **Expansion** – Create a new child node.
3. **Simulation** – Run a playout from the new node.
4. **Backpropagation** – Update the nodes with the simulation result.

---

## 1. Selection

- Starting at the root, MCTS goes through the tree using a selection rule.
- Normally, it uses UCT (Upper Confidence Bound applied to Trees).
- UCT balances two concepts:

**Exploitation**
- Choosing moves with higher average reward.

**Exploration**
- Trying moves with less information.

---

## 2. Expansion

- When selection reaches a non-terminal node with untried actions, a child node is created.
- The child represents the state reached by taking one of those untried actions.

---

## 3. Simulation

- A random playout is run from the new node until a terminal state is reached.
- Actions can be completely random or follow simple heuristics.
- Simple simulations (random or following  simple heuristics) make the process computationally inexpensive.

---

## 4. Backpropagation

- After the simulation, its result is sent back through the parent nodes until reaching the root.
- The number of visits and the value of each node are updated.
- This information helps MCTS make better decisions in future simulations.

---

## Upper Confidence Bound 1 (UCB1)

The first formula used to select nodes was UCB1.

$$
\mathrm{UCB1} =
\bar{X}_i +
c \cdot \sqrt{\frac{\ln(N)}{n_i}}
$$

Where:

- $\bar{X}_i$ = Average reward of node $i$.
- $c$ = Exploration parameter (typically $\sqrt{2}$).
- $N$ = Total visits to the parent node.
- $n_i$ = Number of visits to node $i$.

---

## How UCB1 Works

- The first term represents the average reward.
- The second term represents the exploration bonus.
- The bonus shrinks as a node is visited more.
- $\ln(N)$ grows slowly, so nodes are not abandoned permanently.

**In simple terms:** UCB1 selects the child with the highest average reward plus a bonus that encourages exploration.

---

## Upper Confidence Bound Applied to Trees (UCT)

UCT applies the UCB1 principle to tree search.

$$
\mathrm{UCT}(j) =
\frac{Q_j}{n_j}
+
c \cdot \sqrt{\frac{\ln(N)}{n_j}}
$$

Where:

- $j$ = A child node.
- $Q_j$ = Total reward of all playouts through child $j$.
- $n_j$ = Number of visits to child $j$.
- $c$ = Exploration parameter (typically $\sqrt{2}$).
- $N$ = Number of visits to the parent node.

---

## UCB1 vs. UCT

### Flat UCB
- Bandit-based move selection.
- Does not grow a search tree.

### UCT
- Also uses bandit-based selection.
- Every explored internal node is treated as a separate multi-armed bandit.
- The tree grows as new nodes are explored.

UCT works together with expansion and backpropagation, keeping track of visits and rewards.

As it runs, it gets less probable to fail. With enough time and memory, it would be the same as a minimax tree (exploring all nodes).

---

## Forward Model

- The forward model is a set of rules that lets MCTS know how to play the game.
- It is an approximation of the game logic.
- It allows MCTS to simulate actions and the resulting states.

The forward model tells MCTS which actions are possible and which states they lead to.

---

## Action Pruning

Action pruning can be applied to MCTS to avoid wasting computational resources.

- Removes actions that are not worth taking.
- MCTS can simulate the enemy's moves.
- It can identify states that lead to an enemy victory in two actions.
- It can consider an action redundant if it leads to the same state as another action.

---

## Playouts

A playout takes a node and simulates the game until:

- The game reaches its end.
- There are no more resources available.

Playouts can use different approaches:

- **Heuristic:** Uses rules to guide moves instead of choosing them randomly.
- **Greedy:** Builds moves using a quick rule without searching every option.
- **Deterministic:** The same state always produces the same moves and result.

Heuristics cost more time per playout but can provide better results.

---

## Domain Independence

- MCTS is domain independent.
- It can work in different domains without requiring heuristics.
- It only needs a forward model to know which actions it can take and which states they lead to.
- Heuristics can still improve its performance.

---

## What is Brute Force?

Brute force means trying every possible option, without shortcuts or guessing.

For example:

- 200 possible actions in every state.
- 100 moves in a game.
- 200 is the branching factor.

The total number of possible sequences is:

$$
200^{100}
$$

Brute force would have to explore all these possible sequences (~$1 \cdot 10^{230}$).

---

## MCTS vs. Minimax

### Minimax
- Uses brute force down to a depth limit.
- Expands every state at each level.
- Can become too computationally expensive when the branching factor is large.

### MCTS
- Selects one state to expand at a time.
- Focuses on promising moves.
- Can avoid wasting resources on unnecessary states, such as forced losses.

---

## AlphaGo: MCTS in Practice

For a long time, it was thought that a program could not beat a professional Go player.

Reasons:
- A 19 × 19 board has 361 possible moves at the first state.
- The branching factor is enormous.
- Evaluating Go positions is difficult.

---

## AlphaGo's Achievements

- AlphaGo won 494 out of 495 games (99.8%) against other Go programs.
- In October 2015, a program first defeated a professional in full-sized Go without a handicap, against European champion Fan Hui.
- In 2016, Google DeepMind's AlphaGo defeated world champion Lee Sedol.

AlphaGo combined:

- Neural networks.
- Reinforcement learning.
- Monte Carlo Tree Search.

---

## Generative AI Used

Claude was used to:

- Explain what a forward model is.
- Clarify the difference between UCB and UCT.
- Check whether sources were peer-reviewed.
- Find three peer-reviewed sources.
- Verify possible misunderstandings in our notes.
- Improve our notes.
- Verify and correct information in our PowerPoint.
- Check prototype logs and identify possible problems.

ChatGPT was used to:

- Create the base of this presentation.

---

## References (1/2)

1. GeeksforGeeks, "Monte Carlo tree search (MCTS) in machine learning."  
   https://www.geeksforgeeks.org/machine-learning/monte-carlo-tree-search-mcts-in-machine-learning/

2. Fullstack Academy, "Monte Carlo tree search (MCTS) tutorial."  
   https://www.youtube.com/watch?v=Fbs4lnGLS8M

3. AI and Games, "AI 101: Monte Carlo Tree Search."  
   https://www.youtube.com/watch?v=lhFXKNyA0QA

4. First Principles AI, "Monte Carlo Tree Search (MCTS) explained with detail example."  
   https://www.youtube.com/watch?v=reoP5usaYU4

5. T. Kurbiel, "The animated Monte-Carlo tree search (MCTS)."  
   https://medium.com/data-science/the-animated-monte-carlo-tree-search-mcts-c05bb48b018c

---

## References (2/2)

6. M. A. F. Roxo, "MCTS-based planning for grand strategy games."  
   https://fenix.tecnico.ulisboa.pt/downloadFile/1126295043839104/94042-Manuel-Roxo_resumo.pdf

7. C. Browne et al., "A survey of Monte Carlo tree search methods."  
   https://turing.iimas.unam.mx/~luis/cursos/IA2022-1/lecturas/SurveyMCTS.pdf

8. L. Kocsis and C. Szepesvári, "Bandit based Monte-Carlo planning."  
   https://www.researchgate.net/publication/221112399_Bandit_Based_Monte-Carlo_Planning

9. D. Silver et al., "Mastering the game of Go with deep neural networks and tree search."  
   https://www.cs.princeton.edu/courses/archive/spring16/cos598F/Google-go-nature16.pdf

---

# Thank You

### Questions?