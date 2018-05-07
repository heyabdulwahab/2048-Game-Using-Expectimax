## 2048 Game Using Expectimax
Some time back, I saw a friend playing the 2048 game. One go at it, and I have been hooked on ever since. Sadly though, I haven't been able to get the 2048 tile and win it. It was really frustrating, so I decided to write an **artificial intelligence** to solve the game. In this post, I will discuss my approach for writing the solver.

I just had to write the AI, and not implement the whole game from scratch. [The source code for the AI is in this repository](https://github.com/Wahab16/2048-Game-Using-Expectimax).

### Goal of the game
2048 is played on a 4 X 4 grid, with four possible moves up, left, down and right. The objective of the game is to slide numbered tiles on the grid to combine them and **create a tile with the number 2048.**

There are **four possible moves** from which a player can choose at each turn. After each move, **a new tile is inserted into the grid at a random empty position.** The value of the tile can be either 2 or 4.

From the source code of the game, it was clear that the **value is 2 with a probability 0.9.**
### AI algorithm
I used a depth bounded expectimax search to write the AI. At every turn, the AI explores all the four possible directions and then decides the best one. The recurrence relation that it follows is
<p align="center">
  <img src="http://iamkush.me/content/images/2015/10/CodeCogsEqn--1-.gif" alt="Screenshot"/>
</p>
The **max node** is the one in which the **player chooses a move** (out of the four directions), and the **chance node** is the one in which the **board inserts a random tile**. The max node decides the correct move by maximising the score of its children, which are the chance nodes.

Due to space and time restrictions, it is obviously not possible to explore the complete search space. The **search is bounded by a depth**, at which the AI **evaluates the score of the state** (the leaf node) using some heuristic.
### Screenshot
<p align="center">
  <img src="https://cloud.githubusercontent.com/assets/1175750/8614312/280e5dc2-26f1-11e5-9f1f-5891c3ca8b26.png" alt="Screenshot"/>
</p>
