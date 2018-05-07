# 2048 Game Using Expectimax
Some time back, I saw a friend playing the 2048 game. One go at it, and I have been hooked on ever since. Sadly though, I haven't been able to get the 2048 tile and win it. It was really frustrating, so I decided to write an **artificial intelligence** to solve the game. In this post, I will discuss my approach for writing the solver.

I just had to write the AI, and not implement the whole game from scratch. [The source code for the AI is in this repository](https://github.com/Wahab16/2048-Game-Using-Expectimax).

## Goal of the game
2048 is played on a 4 X 4 grid, with four possible moves up, left, down and right. The objective of the game is to slide numbered tiles on the grid to combine them and **create a tile with the number 2048.**

There are **four possible moves** from which a player can choose at each turn. After each move, **a new tile is inserted into the grid at a random empty position.** The value of the tile can be either 2 or 4.

From the source code of the game, it was clear that the **value is 2 with a probability 0.9.**
## AI algorithm
I used a depth bounded expectimax search to write the AI. At every turn, the AI explores all the four possible directions and then decides the best one. The recurrence relation that it follows is
<p align="center">
  <img src="http://iamkush.me/content/images/2015/10/CodeCogsEqn--1-.gif" alt="Screenshot"/>
</p>

The **max node** is the one in which the **player chooses a move** (out of the four directions), and the **chance node** is the one in which the **board inserts a random tile**. The max node decides the correct move by maximising the score of its children, which are the chance nodes.

Due to space and time restrictions, it is obviously not possible to explore the complete search space. The **search is bounded by a depth**, at which the AI **evaluates the score of the state** (the leaf node) using some heuristic.

## Evaluating a leaf node
I used a combination of two heuristics to evaluate how **good** a state was.
  ### 1. Using a weight matrix
  In most of the games that I came close to winning, the bigger tiles were around the corner. Hence the first idea that I used was to **push the higher value tiles to one corner** of the grid. For this, I assigned different weights to different cells.
  
  This had another advantage, the new randomly generated tiles got generated in the opposite corner, and hence were closer to the smaller value ones. This **increased the chances of the newly generated tiles getting merged.**
  
  ### 2. Forming clusters of equal valued tiles
  It is obviously better for us if more tiles get merged. This happens if in a state two same valued tiles are present next to each other.
  I calculate another value, a **penalty**, and subtract it from the score calculated from heuristic one. 
  
  This penalty becomes large when high value tiles are scattered across the grid, hence indicating that that particular state is bad.
  
## Optimisations of search
As far as I know, there is no method to prune an expectimax search. The only way out is to avoid exploring branches that are highly improbable, however this has no use for us as each branch has an equal probability (the new tile has equal probability of popping up on any of the empty tiles).

Initially I was exploring nodes even if the move played by the PLAYER had no effect on the grid (i.e. the grid was stuck and could not move in a particular direction, which is a common occurrence during gameplay). Eliminating such branches did enhance the performance a bit.

## Performance of the AI
The solver performs quite well. With a **search depth of 6**, it formed the 2048 tile **9 times out of 10.** With a **search depth of 8, it formed the 2048 tile every time** I tested it, and went on to 4096 every time as well.

For a better winning rate, and taking the time taken to search per move into consideration, I keep a 8 ply lookahead if the number of empty tiles in the grid is less than 4, otherwise a 6 ply lookahead. This combination leads to a win every time, and 8 out of 10 times forms the 4096 tile.

**The AI achieved a best score of 177352, reaching the 8192 tile. The average score was around 40000, and the AI always won the game.**

## Screenshot
I forgot to take the screenshot of the best run, but a closer one's here.

<p align="center">
  <img src="http://iamkush.me/content/images/2015/11/Screen-Shot-2015-11-03-at-3-21-44-am-1.png" alt="Screenshot"/>
</p>

Each move takes anywhere from 10ms to 200ms, depending upon the search space and how complex the grid is at that moment. The version here has a timeout function, calling the AI to find the next move every 50 ms.

It can perform better if the search depth is more, however that would make it quite slow. The fact that the code is in javascript doesn't makes things any better :D. 

Writing the artificial intelligence was quite a lot of fun, and I learnt a lot during the process. Do let me know what you think about it in the comments section!

Regards, 
Abdul Wahab

## Donations
I made this in my spare time, and it's hosted on GitHub (which means I don't have any hosting costs), but if you enjoyed the game and feel like buying me coffee, you can donate at my BTC address: 1Gsq2zYnz9JsoMyCsckTz9S9MjGcnsGrSs Thank you very much!
