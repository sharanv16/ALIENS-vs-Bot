# ALIENS-vs-Bot
Avoid aliens as you reach the goal state

You are the space roomba on a deep space salvage vessel, and your best friend is the captain. It’s just the two of you
out here, against the world / the infinite void of space. Unfortunately your ship has been boarded by aliens. The
captain is hiding under the floor panels, but it’s up to you to get to them and teleport you both to safety - avoiding
the aliens as you do.
1 The Ship
The layout of the ship (walls, hallways, etc) is on a square grid, generated in the following way:
• Start with a square grid, D × D, of ‘blocked’ cells. Define the neighbors of cell as the adjacent cells in the
up/down/left/right direction. Diagonal cells are not considered neighbors.
• Choose a square in the interior to ‘open’ at random.
• Iteratively do the following:
– Identify all currently blocked cells that have exactly one open neighbor.
– Of these currently blocked cells with exactly one open neighbor, pick one at random.
– Open the selected cell.
– Repeat until you can no longer do so.
• Identify all cells that are ‘dead ends’ - open cells with one open neighbor.
• For approximately half these cells, pick one of their closed neighbors at random and open it.
Note: How big an environment D you can manage is going to depend on your hardware and implementation, but you
should aim to generate data on as large an environment as is feasible.
2 The Bot
The bot occupies an open cell somewhere in the ship (to be determined shortly). The bot can move to one adjacent
open cell every time step (up/down/left/right). If the bot enters a cell occupied by an alien, or an alien enters the
bot’s cell, the bot is grabbed, and fails its mission.
3 The Aliens
K many aliens are initially randomly placed at open cells throughout the ship. At every time step, the aliens each
can choose to move, up/down/left/right. The aliens can be advanced according to the following steps:
• Order the aliens at random.
• In the selected order, for each alien, choose an action at random that doesn’t move that alien into a blocked
or alien-occupied cell. If no action is feasible, that alien may stay in place. This is the only instance where an
alien may stay in place.
1
Computer Science Department - Rutgers University Spring 2024
• Move the selected alien according to that action.
• Process each alien in this way, in the selected order.
Note, the order should be re-selected at random every time the aliens are advanced. This simulates the fact that the
aliens are choosing independently how to move each time.
4 The Captain
The Captain will be hiding at a random open cell. It does not matter if the cell is occupied by aliens, the Captain
is effectively hiding in the floor or ceiling. The bot’s goal is to make it to this cell, at a time when the cell does not
have an alien in it. If the bot successfully enters this cell on its turn, the bot and the Captain are teleported to safety
and the simulation ends.
5 The Task
At time t = 0, first place the bot, then the K aliens at random, distinct open cells (chosen uniformly from the available
open cells). Additionally, place the Captain at a non-bot-occupied open cell. At each time step t = 1, 2, 3, 4, . . ., the
following happens in sequence:
• The bot decides which open neighbor to move to.
• The bot moves to that neighbor.
• If the bot enters the Captain’s cell, they teleport away and the simulation ends.
• If not, the aliens then advance.
• Then the aliens advance.
• If at any point, the bot enters an alien-occupied cell, or an alien enters a bot occupied cell, the task fails and
the simulation ends.
Note that there are two goals we can potentially use to evaluate the quality of a bot:
• Is the bot able to save the Captain?
• Is the bot able to avoid the aliens?
These goals can potentially be orthogonal - the bot could survive and avoid the aliens for a very long time by just
staying in place or moving away from the nearest aliens, but this strategy would not lead to saving the Captain in a
reasonable amount of time. For a given bot then, we can evaluate the quality of the bot on two metrics:
• As a function of the number of aliens K, how often (on average) is the bot able to save the Captain within
1000 time steps, for randomly generated ships and initial positions.
• As a function of the number of aliens K, in instances where the bot does not save the Captain in 1000 timesteps,
how often (on average) does the bot survive for the duration?
Note that the second metric is inherently conditional.
2
Computer Science Department - Rutgers University Spring 2024
6 The Bots
At any time, the bot can choose from the following actions: move to a neighboring open cell, or stay in place. The
bot must make a choice, based on some assessment of the current arrangement of the ship (walls, bots, and Captain),
and knowledge of how the future might unfold. The thing that differentiates the strategies is how the bot decides
what to do.
For this assignment, you will implement and compare the performance of the following strategies.
• Bot 1 - This bot plans the shortest path to the Captain, avoiding the current aliens, and then executes that
plan. The movement of the aliens is ignored as the bot executes this plan. Note: This is a terrible plan, and
should only be effective when there are few aliens, and they are very far away. This bot serves primarily as a
baseline for comparison for the other bots.
• Bot 2 - At every time step, the bot re-plans the shortest path to the Captain, avoiding the current alien
positions, and then executes the next step in that plan. Because it repeatedly replans, it constantly adapts to
the movement of the aliens.
• Bot 3 - At every time step, the bot re-plans the shortest path to the Captain, avoiding the current alien
positions and any cells adjacent to current alien positions, if possible, then executes the next step in that plan.
If there is no such path, it plans the shortest path based only on current alien positions, then executes the next
step in that plan. Note: Bot 3 resorts to Bot 2 behavior in this case.
