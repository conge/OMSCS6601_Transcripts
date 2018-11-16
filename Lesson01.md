# [Lesson 1: Game Playing](https://classroom.udacity.com/courses/ud954/lessons/4747398595/concepts/last-viewed)
## Title 01 - Course Introduction 

Hi, my name is Thad Starner. At Georgia Tech, I teach the AI course, the mobile ubiquitous computing course, and when I'm lucky, the computer science rapid prototyping hardware class. In this course, we're taking parts of Sebastian Thron's and Peter Norwick's AI course on Udacity and mixing it with the best parts of my teaching at Georgia Tech. When Sebastian and Peter first presented the idea of this online courses, one hope was that we could mix the best parts of the best courses of top lecturers and make a class which is better than what anyone of us could do. Peter will be covering search and logical planning. Sebastian will be teaching the section on probability and base nets. And I'll be covering the rest of the content. Let's get started. 

## Title 02 - Overview  

Your goal in this lesson is to design an AI that is smarter than yourself. At least in playing a real game. Now granted, a single game is a very limited context. But making a program that can play a game better than you can is an exciting introduction to AI. It's the way I got hooked. I'm hoping to share my enthusiasm. Along the way, you will learn how to use adversarial search to find optimal moves. In particular, we will talk about the minimax algorithm and alpha-beta pruning. We'll also talk about how to create evaluation functions for games. These ideas are enough to create surprisingly powerful game AIs. We will apply our knowledge to Isolation, a complex game with surprisingly simple rules. We will then examine multi-player and probabilistic games. As your assignment for this lesson, you will need to create a game player for Isolation. Note how long it takes you and how difficult you find it. That should give you a sense of the amount of time and effort that this course will require. 

## Title 03 - Isolation 

Let's dive in and start with something fun. Let's teach you the rules of isolation, the example game we'll be using. Shelly and I will play a game of isolation on a five by five board. I'll be O and move first. O gets to place his piece anywhere on the game board. I'll choose here. Shelly will be X. And she can place her piece anywhere remaining. Shelly, where do you want to go? 
&gt;&gt; I'll go here. 
&gt;&gt; From now on, our players move like queens in chess. For example, for my next move I can go any square that's horizontal or vertical or diagonal from me, except I can't move through Shelly's piece here. I only have this diagonal move. 
&gt;&gt; Okay, I get how players move. But do they attack each other like in chess? What's the objective of the game? 
&gt;&gt; The objective is to be the last player to move. The players cannot go outside the boundaries of the game board. Nor through a position that is currently or was previously occupied. The first player to get isolated, and by that I mean, unable to move on their turn, loses. For my next move I think I will move here. 
&gt;&gt; Wait a second. Why aren't the squares between the start and the end position filled in? 
&gt;&gt; You mean these squares here? You're thinking about the game, Snake. In Isolation, it's just where you land that becomes unobtainable for future moves. 
&gt;&gt; Okay, in that case I'll move here. 
&gt;&gt; Great so now we have a game going. So lets see here. I want to go here and limit your moves for the future. 
&gt;&gt; Okay, so I'll go here. 
&gt;&gt; Cool, I think that what I'll do is try to force an early end to the game. I'm going to do this and start dividing up the board. 
&gt;&gt; Okay, so there's more spots filled up on the right side of the board. So I think I'll go towards the left so I have more space to move in. Okay, that was a wise move. Let me actually go here and see if I can trap you in. 
&gt;&gt; Okay, so I'm going to keep going left and see if I can escape, maybe get back on to the right side. 
&gt;&gt; Okay, I want to prevent you from getting out, so I think I can do that. I think I can actually win at this point, so I'm going to go here and stop your diagonal move out. 
&gt;&gt; I look like I'm stuck [LAUGH] I guess I have to try to get out. 
&gt;&gt; We're getting to end game pretty quickly here because of that immediate move where I could partition the board. So I'll go up here and now you're stuck and now since you have no move, I win. Yehey, okay, so what we've shown here is basically, the rules of isolation and the game board. We've also shown some tricks that show about the difficulty of the game and how you can early on create a partition that the device the boards up on the one side of the other and hang them up then fight for their correct side. Strategy for isolation is really quite complex. You'll find out pretty quickly when you actually try it on your homework. 

## Title 04 - Building a Game Tree 

Now let's use a simple two by three version of isolation to illustrate how to make a game tree that shows all the moves possible during a game. We will use this game tree to select moves which give the best guarantee of winning. Consider any field spot on the game board as unavailable. We will start with one field spot. We'll call the beginning game level zero in our game tree. Assume that O is the computer player. O is the first to move, so the computer has five choices in the beginning. We draw out all the possible boards for level one of the game tree. X goes next and can put its piece anywhere on the game board that has not been taken by O. So no matter which position O occupied for its first move, X has four choices. We can quickly draw the nodes for level two of our tree. From then on, the number of choices for each player becomes smaller as the pieces are limited to moves like a queen in chess. O goes next. To the left most leaf, O can move straight down. [BLANK_AUDIO] Diagonally down to the right. But it can't move through X. 

## Title 05 - Which of These Are Valid Moves

Shelly, why don't we have a quick quiz on isolation? 
&gt;&gt; Sure. So for each of these board states, assuming it's O's turn, select the valid positions to which O can move 

## Title 06 - Which of These Are Valid Moves_ Solution 

Okay, so remember that the pieces can move like a queen in chess, but can not pass through positions that have been previously played. Also, when a piece is moved it doesn't leave a trail like the game snake, but just the last position is marked as impassable. So here are those resulting child nodes. [BLANK_AUDIO] 

## Title 07 - Building a Game Tree (Contd.) 

Let's keep expanding the nodes in order until we have fully expanded level three of our tree. As we see some nodes have two children. Others have three. Even so we see that the exponential growth of the children makes it an increasingly difficult to display the full tree on the screen at once. In level four, it is x's turn. Again, we expand the tree completely. Scrolling through the tree shows how increasingly dense it is getting. Level five gets a bit more interesting. In the left-most branch, o has one final move. However, in the next branch x has blocked o in. O has no move and the game ends. To indicate that our computer player has lost in this situation we will mark this node with a -1. Expanding level five completely we see that one branch looks particularly dire for a computer player. As it has many losing situations. Even worse, the opponent has control of the board at the level above. We have to find a way to warn our computer player from making a move that leads to this branch. By level six there are no moves left anywhere on the board. All games have ended. The ones where o wins, we will mark with a plus one. The ones where o lost on level five, we have already marked with a -1. Now that we have seen all the possible futures of the game. We will back propagate this knowledge to our computer player so it can make a wise first move. 

## Title 08 - How Do We Tell the Computer Not to Lose

In most situations in this game tree our computer player O wins. But in a few situations,the game ends with our opponent X winning at level five. If our computer player makes a poor choice for its first move, X can win no matter what O does. Clearly we never want our computer player to make this bad first move. To avoid that situation we can keep a table of best first moves for this board configuration. This table to called an opening book, similar tables can be kept for end games. But how do we make discovering these bad moves automatic? Better yet how do we discover the best moves for a computer player? The answer is the mini max algorithm, a version of which we will see next. 

## Title 09 - MIN and MAX Levels 

With terminal node in our game tree. With a +1 if our computer player wins and a -1 if it loses. We've used squares to indicate the terminal nodes and put their scores inside them. We are going to assume that our opponent x is really smart and will always play to win. In other words, we'll assume that x will always minimize o's scores. While our computer player will always try to maximize its scores. To show that idea, we are going to mark the game tree with a triangle pointing up to indicate turns when our computer player is trying to maximize the score. The triangle pointing down when the component is trying to minimize the score. Since our computer player is going first, we start with the maximization level. In general, since we are only searching from our computer player's perspective, they can only affect the game when it is its turn, the top of a minimax tree is always going to be a max level. 

## Title 10 - Propagating Values Up the Tree 

In order to propagate wins and losses up the game tree, to the point where a computer player can make a decision, we need to start at the bottom of the tree. Let's start with the left side of the tree. In the left-most branch, O wins. In the next branch over, O never gets to use the last square on the game board and loses. Since our opponent x plays to win and minimizes O's score, it will never choose the left most branch. Instead it will choose this branch which is guaranteed to give the computer player a score of -1. However, our computer player can avoid this situation by making a wiser choice one level up. Both branches of this tree lead to victory, hence scores of +1. We can keep propagating these scores up the tree, alternating men in max levels until we arrive at the top. This process of computing values for each node bottom to top is known as the mini max algorithm. For each max node, pick the maximum value among its child nodes. If there's at least one plus one child, the computer can always pick that to win. Otherwise, it could never win from that point on, assuming a perfect opponent. Of course, through represent the opponent, at each mid node, we pick the minimum value. 

## Title 11 - Computing MIN MAX Values 

Shelly, I think I've talked for too long. It seems time for a quiz. 
&gt;&gt; I agree. So Thad filled in the left side of this tree. Try to fill the rest of the values on the right side. 

## Title 12 - Computing MIN MAX Values Solution 

Here are the rest of the values for the tree. [BLANK_AUDIO] 

## Title 13 - Choosing the Best Branch 

Thanks, Shelly. Now we can see that our computer player loses in only two branches, assuming otherwise perfect play. All our computer player has to do is choose any of the other branches and it will win. 

## Title 14 - Aside_ Reading the Book 

Y'all can think of Shelley and me as your guides on a whirlwind tour of AI. These lectures are designed to show you what we feel are the most important stops on the tour, and to give you some context and intuition. 
&gt;&gt; We also hope to peak your curiosity and get you excited about the material. 
&gt;&gt; However, real understanding will require reading the book and the papers we provide. As we go we'll point out the relevant readings. 

## Title 15 - Max Number of Nodes Visited 

So far we've worked with an isolation board with only five possible moves. It's searching to end game caused huge gain trees that were hard to show on the screen. How bad will it get with an interesting game board? Well let's pull up the five by five board Shelly and I were playing on. How many different leaf nodes can we expect? 
&gt;&gt; Well it's easy to put an upper limit on the number of nodes. 
&gt;&gt; How so? 
&gt;&gt; On the first move, O can put its piece in any of 25 places. Then on the next move, x can put its piece in any of 24 places. After that, there are 23 empty places followed by 22 and so on. Now all of these spots are always possible moves, so we know that there have to be less nodes than 25x24x23, and so on. So less than 25 factorial nodes. 
&gt;&gt; 25 factorial, how big is that? 
&gt;&gt; About 10 to the 25th power. 
&gt;&gt; Okay, if we assume a moderate multi-core gigahertz processor which can do 10 to the 9th operations per second, it will take 10 to the 16th seconds to search the entire game. Okay, each hour is 36 hundred seconds and each day is 24 hours and each year has 365 days. That means over 300 million years. 
&gt;&gt; Actually 317,097,920 years to be precise. 
&gt;&gt; I don't think we have that long to wait. 
&gt;&gt; I just gave you a quick upper bound estimate of the number of nodes. It isn't really that bad. 
&gt;&gt; You're right. Let's see if we can do a better estimate. Okay, there's not much we can do about the first moves. There really are 25x24 potential first moves. But after that, each piece moves like a queen and there are less moves. What is the maximum number of moves possible in the third move of the game? 
&gt;&gt; That sounds like a great quiz question. 

## Title 16 - Max Moves 

Select the position on the board with the maximum number of options to move next once the piece is limited to moving like a queen in chess. In other words, the third move in the game. Assume that the first move would not block you. 

## Title 17 - Max Moves Solution 

Assuming that the opponent is not in the way, the center position has a maximum potential moves with 16. Two vertically up. Two diagonally to the right. Two horizontally to the right. Two diagonally down to the right. Two down vertically. Two diagonally down to the left. Two to the left horizontally. And two diagonally up to the left. 

## Title 18 - The Branching Factor 

While the center position has 16 spaces in which it can move, most other positions have 12 or less. As the game plays, we will have less and less spaces our player can move. 
&gt;&gt; That means we can make a better estimate on how many nodes we have right? 
&gt;&gt; Yep, okay, we know we cannot have more than 25 moves in a game. That is the maximum depth our tree is going to be. And we already know how many moves can be done in the beginning, which leaves 23 moves left. We know that each move after the first two are generally going to have 12 or fewer moves available. We'll call this the branching factor. In worse case, we can expect 25 times 24 times 12 to the 23rd n nodes in our game tree. Shelly, how many is that? 
&gt;&gt; More than 10 to the 27th power. 
&gt;&gt; Ouch, okay that really didn't help. Perhaps we can improve our estimate more. 
&gt;&gt; Well, we know at the end of the game there have to be progressively fewer moves available. There are zero moves in the end, one move before that, a maximum of two before that, three before that, and so on. 
&gt;&gt; You're right. So for the last few moves, assuming a branching factor of 12 is way too much. That would be around 10 to the 13th nodes. But the maximum it could be is 12 factorial, or about 5 times 10 to the 8th. I guess we could redo our estimate as 25 times 24 times 13 12s in a row, times 5 times 10 to the 8th. That's approximately equal to 3 times 10 to the 23rd. That is better than 10 to the 25th, or the 10 to the 27th, but still seems like a gross overestimate as most branches will have much less than the maximum branching factor. 
&gt;&gt; Let's see if the students have a better answer than we do. 

## Title 19 - Number of Nodes in a Game Tree 

How many nodes do you think the Minimax algorithm will need to visit in the game tree? Here's some choices where b is the average branching factor, and d is the average depth. So we have bd, d to the power of b, d squared, and b to the power of d. Pick what you think is the best answer. 

## Title 20 - Number of Nodes in a Game Tree Solution 

We can approximate the number of nodes MINIMAX will need to visit using the formula b to the power of d. 

## Title 21 - The Branching Factor (Contd.) 

What if we do average branching factor instead? 
&gt;&gt; That sounds like a good idea but, how do we do that? 
&gt;&gt; Just try it out. 
&gt;&gt; Sure, why not? I always say do the simple thing first and only add intelligence when necessary. Let's write a quick computer game player that moves randomly and just test see what our branching factor is on average. [BLANK_AUDIO] 

## Title 22 - Max Number of Nodes 

Okay, after playing with five by five isolation for a bit. The average branching factor seems to be about 8. So we now get an estimate of 8 to the 25th nodes. 
&gt;&gt; That's about 10 to the 22nd nodes. 
&gt;&gt; Great, that means we only have to wait for around 1.2 million years to get our answer. 
&gt;&gt; Yeah, you're not going to be alive by then. 
&gt;&gt; Them dang kids, no good 
&gt;&gt; Thanks. 
&gt;&gt; No problem. 
&gt;&gt; Okay. That means we need to be more clever about how we make a computer player for isolation. The exponential growth of the game tree means we can't brute force the problem and search for the end-game easily. In general, more interesting games will not be searchable til the end. Either the branching factor is too large, or the depth, or both. When the number of nodes, which we can estimate by branching factor to the depth power. Starts becoming comparable to the number of seconds remaining in the life of the universe. We know we are in trouble. 

## Title 23 - Depth-Limited Search 

Waiting for a computer game player to make a turn is no fun. After two seconds of waiting, a human player is likely to start thinking of other things. We really need a way to choose a move quickly. 
&gt;&gt; Well, we can't search to end game but how can we search? 
&gt;&gt; Let's assume again that we can search ten to the ninth nodes per second. So in two seconds we can actually search two times ten to the ninth nodes. So we need to solve the equation eight to the X is less than two times ten to the ninth. We can do this by taking log base eight on both sides. 
&gt;&gt; That log base eight is pretty annoying. 
&gt;&gt; Yup, time to bring out some old math skills. We know that log A of X is equal to the log B of X over the log B of A. So we can use that formula to solve our problem. We'll use log base ten here for convenience. So we end up with X is less than log base ten of two times ten to the ninth over log base ten of eight. And we end up with X being less than 10.3. 
&gt;&gt; So we should be okay if we only search ten levels deep? 
&gt;&gt; Provided that our estimate of a branching factor of eight on average is good. To be on the safe side let's only go to depth nine. This idea of only going as far as we think we can to safely meet our deadline is called a depth limited search. 

## Title 24 - Evaluation Function Intro 

Okay, great, but what do we do when we get to level nine in our mini-max tree? 
&gt;&gt; That's when things really get interesting. We want to evaluate the goodness of a node at level nine based on how much we expect it to lead to a win for our computer player. Can we create an evaluation function that takes in each game board generated level nine of our mini-max game tree, return a number that we can use to compare that node for all the other nodes at that level? 
&gt;&gt; Well, we know the only way to win is have our computer player have moves left at the end of the game. Maybe our computer player shouldn't maximize the number of moves it has? 
&gt;&gt; That sounds right. We want an evaluation function that returns a higher number depending how good the board is for our computer player. With an evaluation function like simply counting the number of moves our computer player has available at a given node, the player will select branches in the mini-max tree that lead to spaces where a player has the most options. It seems like a really good idea. Let's call that evaluation function, number my moves for convenience. Why don't we test it out on our simple five-move game board and see how well it works? 
&gt;&gt; Let's do it. 

## Title 25 - Testing the Evaluation Function 

For this mini max sub-tree, fill in the values. Assuming an evaluation function of number of my moves or the number of moves possible for player O. Fill in the numbers starting at the bottom and then propagate them up to the top of the tree. 

## Title 26 - Testing the Evaluation Function Solution 

The bottom level is easy to compute. It's just the number of moves available to player O. Then the second level from the bottom is a max level. So we take the max at each branch here. Then the top level of this subtree is a min level. And we end up with a 2. 

## Title 27 - Testing the Evaluation Function Part 2 

Now try filling out the values for the entire minimax tree. Again, using the number of my moves evaluation function. Propagate the numbers to the top of the minimax tree. Since we can't fit the entire level three tree here, we'll use this abbreviated version instead. A link to the full tree has been provided in the instructor notes. Open it up and compute all the values, starting at the bottom and propagate them up to the top. Then report back the values for a level one and the root node here. 

## Title 28 - Testing the Evaluation Function Part 2 Solution 

So, here are the values that I got. There's not much variation here, but there's a number of good choices. You can see three branches that have a value of 2. Our computer player will take the one on the left automatically, so we'll probably take this move to start with 

## Title 29 - Testing Evaluation Functions 

Remember when we had our simple five-move isolation board? And we searched it to end-game? We found that player one would always win unless it moved to one of the center positions first. 
&gt;&gt; I see where you're going. The question is whether our evaluation function of #my_moves predicts which branches will lead to a win and which ones will lead to failure? 
&gt;&gt; Exactly. I always like to test my assumptions because they are often wrong. 
&gt;&gt; Okay, so here's the mini-max tree where I have loaded the the depth to three and applied the evaluation function. I've also propagated the values up the mini-max tree to the top. 
&gt;&gt; The result looks promising. The two center positions have scores of 1, while the rest of the branches, which we know when, have higher scores. Let's try another example. What happens if we try the same thing but assume we could only go two levels deep on our search tree? 

## Title 30 - Testing the Evaluation Function Part 3 

Now, let's assume we can only search to level two of this minimax tree. Fill in the values of the bottom of the tree, again assuming the evaluation function of number of my moves. Propagate the numbers to the top of the minimax tree. As in the previous quiz, use the complete level two tree linked in the instruction notes, then fill out the top two levels here. 

## Title 31 - Testing the Evaluation Function Part 3 Solution 

Here are the values we get after searching to level two of our game tree. We can see that the values are significantly different from when we searched to level three. Previously, we would have chosen one of these three nodes, because they received the highest value in our evaluation function. But after only searching to level two, we see that it's actually the other nodes that get a higher value in our evaluation function. So, in this case, our computer player would choose one of these two moves. And if it takes the move on the left first, it would choose this move, which is a different choice than he would have made after surging to level three. 

## Title 32 - Quiescent Search 

With the depth limit of two we get a very different result than with the limit of three. 
&gt;&gt; Yep, at level two, the losing center moves. Both have scores of three, while the winning moves have smaller scores. 
&gt;&gt; Does that mean our evaluation function is bad? 
&gt;&gt; Not necessarily. It may mean that we're not searching deep enough to get good answers. 
&gt;&gt; How do we tell? 
&gt;&gt; One way to check is to see which branches the minimax algorithm recommends at each level of the search. If the results vary widely between certain levels, it may be because critical a decision is being made at those levels. Let's try it out on the example isolation game. We will continue to use the number of my moves evaluation function. Which can have a maximum score of five and a minimum score of zero. If we find a branch where our computer player loses, we'll give it a score of -1. If we find a branch where our computer player wins, we'll give it a score of 100. With this example isolation game at level one, the number of my moves evaluation function would recommend the two center moves and the top left move. Each of these have scores of four. At Level 2,the two center moves are considered the best with scores of 3. At Level 3, the center moves are considered worse. That continues for Level 4, 5 and 6. [BLANK_AUDIO] After level three, we've reach a state of quiescence. The recommended branches are not changing much. We know we are in good shape. [BLANK_AUDIO] 

## Title 33 - A Problem 52

But that doesn't help us. 
&gt;&gt; Well, why not? 
&gt;&gt; Well, the whole point of limiting the depth of our search and using an evaluation function was to avoid the exponential explosion in the number of nodes we have to examine. With quiescence search we have to search the game three for two levels and maybe a whole lot more if the results are changing between levels. 
&gt;&gt; Well, we don't have to use quiescence search all the time. In fact, if we play isolation a lot and see that the evaluation function is giving us consistent results, then we might not bother at all. However, we might find that in our testing that certain times in the game creates quiescent search might give us some better results. Often times, that's at the beginning of the game, or at the end of the game. Also we have another trick we can use called iterative deepening, and that might help us too. 

## Title 34 - Iterative Deepening 

What's iterative deepening? 
&gt;&gt; Let's go back to our problem of making our computer respond to an opponent in two seconds or less. Before, we calculated the maximum depth we thought we could search in time safely. With iterative deepening, we are going to do something simpler. We are going to search the Level 1 and get an answer for what we think is the best move. We'll keep that answer in case we run out of time. But we'll start the process again and go to Level 2 this time. If we finish searching Level 2 before time runs out, we'll keep its best move and restart the search from going from Level 3. We'll continue doing this process until we run out of time and then return our best answer. 
&gt;&gt; So you're saying we should do the same thing we did to determine quiescence? 
&gt;&gt; Basically quiescence is a good side effect. 
&gt;&gt; But that still doesn't answer my question. Isn't this process inefficient? 
&gt;&gt; Surprisingly, iterative deepening doesn't waste that much time. Because of the exponential nature of the problem the amount of time is dominated by the last level searched. 

## Title 35 - Understanding Exponential Time 

Let's look at a simple case of Iterative deepening on a tree with a branching factor of 2. A level of 0, only one node is searched. At level 1, we touch this first node again and then look at the two child nodes. We've had to explore three nodes for level 1 plus one node from level 0. So, our total for iterative deepening nodes, is 4. At level 2, we add four nodes to the bottom of the tree. To add these nodes, we are looking at the 3 above it as well for a total of seven for this level. Adding level 2 to our running tally is 11 nodes for iterative deepening. At level 3, we had eight more nodes, so we examine 15 nodes for this level in total. Our iterative deepening total is now 26. Know that at each level, the sum total of nodes visited and revisited by iterative deepening is actually under two times the number of nodes expanded by that level. A depth-limited search to level 4 would examine 31 nodes, but the iterative deepening total was 57. Depth limited to level 5 would be 63, and iterative deepening would be 120. While researching the tree may seem like a waste in practice, it is a small multiplier compared to the exponential increase from searching each level deeper. With a branching factor of 2, iterative deepening expands less than two times the number of nodes a depth limited search would have done at the same level. When the branching factor is greater than 2, as in most games, then the redundancy caused by iterative deepening is even less. Perhaps a quick quiz would help. 

## Title 36 - Exponential b=3 

Now let's consider a tree with a branching factor of three. How many nodes are visited at levels zero, one, two, three, and four? Tell us the number of tree nodes, and also the number of tree nodes visited by iterative deepening. 

## Title 37 - Exponential b=3 Solution 

Here's the answer. At level 0, we have 1 node in the tree and iterative deepening has only visited 1 node. At level 1 we now have 4 nodes in the tree. Iterative deepening will have visited 5, the 4 nodes in the tree now and one node from the previous level. At level 2, we now have 13 nodes in the tree and iterative deepening will have visited 18. 13 from this tree plus the 5 from the previous time. At level three we have 40 nodes in the tree plus 18 makes 58 nodes visited binder of deepening. Finally, at level 4 we have 121 nodes in the tree, iterative deepening we'll be visiting 179. Which is actually less than one and a half times the number of nodes in the tree. Remember that before when the [INAUDIBLE] factor of 2, we had 2 to the d + 1 minus 1 nodes in the tree. Now with the project factor of three, we have 3 to the d + 1 minus 1 over 2 nodes in the tree. Let's go ahead and generalize this. For a project factor of k, the formulas K to the d + 1- 1 over K- 1 for the number of nodes in the tree. 

## Title 38 - Varying the Branching Factor 

So basically you're saying that iterative deepening is almost free because of the exponential nature of the problem? 
&gt;&gt; Yeah. 
&gt;&gt; And I bet that iterative deepening really helps when you have a game like Isolation with the wildly varying branching factor. 
&gt;&gt; How so? 
&gt;&gt; Well when we calculated that we should only go nine levels deep in a five-by-five isolation tree to return within two seconds. We assumed a branching factor of eight. Yet toward the end game, Isolation players only have a few moves available. We might be able to search much more deeply at end game. 
&gt;&gt; And often, end game is precisely the time, that you want to search more deeply, so you can see what will happen. 
&gt;&gt; On the other hand at the beginning of the game the branching factor is quite large. Our computer player might not return within two seconds. 
&gt;&gt; Correct. Iterative deepening means that our computer player always has an answer ready in case it runs out of time and it can search as far as possible within its time constraints. We limited our time to two seconds per move. Yet in some games, like speed chess, we limit the total time a player can take for the entire game. In those situations, our computer player will want to search deeper in some parts of the game and shallower in other parts of the game. Often we can create a strategy for how deep we want to search. 
&gt;&gt; So something like having a book of standard initial moves for the beginning of the game, then search deeper in the middle and use less time towards the end. Relying on the reduction and branching factor and iterative deepening to still allow our computer player to search to end game or as deep as it can given the remaining time. 
&gt;&gt; Or we might want to have a conservative amount of time we dedicate per move and use iterative deepening and quiescent search to determine the few moves we wanted to spend extra time. All these are great strategies, but we can still get into trouble due to something called the horizon effect. 

## Title 39 - Horizon Effect 

One remaining challenge in making our computer player are situations where it is obvious to a human player that the game will be decided in the next move, but that the computer player cannot search far enough into the future to figure out the problem. This situation is called the horizon effect. Take a look at this isolation board. It's o's turn, which move should o take? 
&gt;&gt; Well, as soon as o moves diagonally down and left one, x will only have six moves as compared to o's seven. Then o will win. 
&gt;&gt; But that is 13 moves in the future. How did you figure that out? 
&gt;&gt; X will be blocked on the wrong side of a partition and not be able to reach o. Both players will have to fill in the rest of the game board efficiently in order to have a chance of winning. 
&gt;&gt; That's a great observation, but our computer player may not see it. Remember, when we started talking about having only two seconds to make a move, we said that our computer player should only look ahead nine moves. It won't see the endgame coming. 
&gt;&gt; But with iterative deepening it might be able to go deeper. The branching factor towards the end of the game is much smaller. True, however, imagine that we have all ready reached the next to last depth of our search tree in the time we have. We have to evaluate the goodness of the children of this node. Which move would our my moves evaluation function pick? 
&gt;&gt; Well, certainly not the winning move. The evaluation function will return three for the winning move, but I can see a couple moves that would be preferred. 
&gt;&gt; Going the whole way to the right gives a five. 
&gt;&gt; And going diagonally left and down two spaces gives a six, yet both of these moves will result in a loss. 
&gt;&gt; Yep, going to the right will get o on the wrong side of the partition. Going to the left causes o to create another partition with even less moves. 

## Title 40 - Horizon Effect (Contd.) 

So what do we do? 
&gt;&gt; Well, perhaps we should include a check in the evaluation function to see if a partition is formed by the next move. And if so, start counting the number of moves left to each player. 
&gt;&gt; That sounds like a good idea, but how much is it going to cost us? 
&gt;&gt; What do you mean? 
&gt;&gt; Our evaluation function goes from a very simple function to compute to something that may involve a complicated check followed by a lot of counting. Simply counting the number of contiguous squares a player can use is similar to searching more levels down in the search tree. We know that is very time consuming. More to the point, we just doubled the amount of time the evaluation function takes. That time becomes multiplied exponentially due to the branching factor as the search goes deeper. 
&gt;&gt; Ouch! Are complicated evaluation functions always not worth it? 
&gt;&gt; It really depends on the game. Changing the rules of isolation slightly can dramatically change whether it is better to use a simple evaluation function and search deeper. Or use a complicated evaluation function that catches these dangerous situations. I've tried isolation variants like having the players move a knight in chess. Or allowing a wrap around on the borders or even having each player control two pieces. And I'm often surprised at which strategy seems to win. It always pays to think carefully about the evaluation function. Whether it's capturing the desired behavior and how efficiently it can be implemented. 

## Title 41 - Good Evaluation Functions 

Now let's think about some other potential evaluation functions for isolation. Here we have some options. Number of opponent_moves. So similar to the number of my_moves, this is the number of moves your opponent has available. Number of squares_remaining, which is the number of empty squares left on the board. Number of squares_remaining minus number of my_moves. And finally, we have number of my_moves minus number of opponent-moves. So go through and mark all the formulas you think that would make a good evaluation function for isolation. 

## Title 42 - Good Evaluation Functions Solution 

The number of opponent moves would actually do the opposite of what we want. It would label boards as good where our opponent has a large number of moves when we actually want to isolate them and prevent them from moving. Number of squares remaining doesn't reflect the goodness of the board. It's constantly decreasing with each move. Number of squares remaining minus number of my moves would penalize our computer player for boards with more potential moves, which is counter productive. Finally, number of my moves minus number of opponent moves is a good potential evaluation function for isolation. It continues to take into account boards where the current player can make a larger number of moves and also penalizes boards where the opponent can make a large number of moves. 

## Title 43 - Evaluating Evaluation Functions 

Let's talk about a different evaluation function. Let's use number of #my_moves minus the number of my #opponents_moves. I really like variants of this function for simple isolation games. The point of isolation is to illuminate the opponent's moves. #my_moves- #opponents_moves causes the computer player to seek moves with the most options while trying to get in the way of the opponent's moves. We can even weight the components of the formula to try to encourage more aggressive or less aggressive game play. For example, #my_moves- 2 * #opponents_moves will cause our computer player to chase after the opponent. 
&gt;&gt; That makes the examples you gave for the horizon effect much more interesting. 
&gt;&gt; How so? 
&gt;&gt; Well, the winning move now has the highest evaluation function result. Here is the winning move. And the evaluation function now returns a 1. For the move immediately to the right, results in a -2. The move to the far right returns a -1. And the far diagonal move returns a 0. 
&gt;&gt; Maybe that is the answer, maybe keeping your options close, but your enemies closer is the right strategy in isolation. 
&gt;&gt; I'm not so sure. I think the only way to really know is to try lots of variants of evaluation functions and see which ones are the best. 
&gt;&gt; You're right, but in addition to minimax and iterative deepening, there's one more trick we have to teach that can really affect the efficiency of game tree search before we spend time doing the evaluation function. 

## Title 44 - Alpha-Beta Pruning 

Alpha-beta is a pruning technique that allows us to ignore whole sections of the game tree, but still get the same answer as with minimax. 
&gt;&gt; So you're saying that alpha-beta never changes the answer, but is more efficient than minimax? 
&gt;&gt; Precisely, often it is much more efficient. Let's take another look at the minimax tree for level three of the five move Isolation board. We'll assume that our computer player is evaluating the game tree from left to right. We have five subtrees that we'll consider. Looking at the most left one, the first branch has only two nodes with values of 1 and 2 respectively. It is the max level, so we choose the two and propagate it up to the mean level. 
&gt;&gt; Since the opponent x will choose a branch that minimizes the value, we know this sub tree will have a value of 2 or less. So that means for any of the remaining branches, as soon as we see a 2 or more, we could ignore the rest of the nodes because they will never be selected. 
&gt;&gt; Precisely. 
&gt;&gt; And we have 2s in the leftmost node of these three branches. 
&gt;&gt; Yep. 
&gt;&gt; Which means we can ignore 6 of these 11 nodes. Wow, that's going to save some time. 
&gt;&gt; It gets even better. Let's look at the next subtree over. As soon as we get to this two on the left hand branch, we know that this whole subtree is going to return a 2 or less. But we already know that we have a 2 from the first subtree. So at the highest node, we already know that we will get a 2 or more. That means we can ignore all the remaining branches of the second subtree. Going on to the middle subtree, we again get a 2 in the left most node. We don't have any constraints on the node above yet. So we keep exploring and get another 2. No other valid move is possible, so this max node returns a 2. Now, the node above is a minimizing node, so it must return a 2 or less. Thus, because we know that the top most level already has a branch with a 2 value, we can ignore all the rest of the nodes in the subtree. 
&gt;&gt; I see a pattern here. With the fourth subtree, we also get a 2 early on, which means we can ignore most of the nodes in this subtree as well. [BLANK_AUDIO] 
&gt;&gt; And the same thing happens in this last subtree. [BLANK_AUDIO] 
&gt;&gt; Hold it, this sub-tree is one of the bad moves. Its value is actually 1. Don't we want our computer player to know that? 
&gt;&gt; Why? As long as we only care about our computer player selecting the same good move as it would with the minimax app rhythm, we are okay. Here our computer selects the left most branch, which is the same as our minimax arrow. 
&gt;&gt; You're assuming that minimax is valuing left to right and takes the left most branch in case of a tie. 
&gt;&gt; Yep, we're assuming that our goal was to play the game, not keep a list of all the equally good moves on a given level. 
&gt;&gt; That's exciting. Looking at the entire tree, it seems we don't need to look at most of the nodes. We only need to look at 29 of the 78 nodes on the entire tree. That will save a lot of time. 
&gt;&gt; In fact, while our minimax algorithm runs in b to the d time, minimax with alpha-beta pruning can run b to the d over 2 time if the nodes are ordered optimally with the best moves being first. Even with random move ordering, alpha-beta pruning reduces the expected run time which is b to the three-quarters d. 

## Title 45 - Minimax Quiz 

Fill in the values of this minimax tree. Remember, the root node is the max level, then we alternate min levels and max levels. 

## Title 46 - Minimax Quiz Solution 

Here's the answer. We'll start at the bottom and propagate the bottoms up. The first level from the bottom is a max level, so we'll take the max of each of our leaf nodes. Next, we have a min level, so we'll take the min of each of the values we just found. Finally, the top is the max level. 

## Title 47 - Alpha-Beta Pruning Quiz 1 

Now that we've filled in the tree, which nodes can be pruned with Alpha-Beta? So go through and click any of the check boxes for nodes or branches that you think would be pruned by the Alpha-Beta algorithm. 

## Title 48 - Alpha-Beta Pruning Quiz 1 Solution 

Here we can print this node right here. If we only knew the 11, this max level here would be 11 or greater. However, even if the value was greater than 11, the next node up would still take the five senses of mean level. That means that we actually don't need to check the four node because it's irrelevant. And thus we can prune it from the tree. We can't prune any other nodes or branches in this situation. 

## Title 49 - Alpha-Beta Pruning Quiz 2 

Let's consider the same tree as the previous quiz. Is there a way to re-order the leaf nodes of the tree in order to prune as many nodes as possible? You'll find a link to the original version in the instructor notes below this video. Refer to that tree and fill in the values here for your re-ordered tree. Then, go ahead and check the boxes to indicate which nodes or branches you can now prune from the tree. Remember, you can enter an exact value like five or a range like greater than or equal to five 

## Title 50 - Alpha-Beta Pruning Quiz 2 Solution 

Here's the answer. We haven't made any changes to the left side of the tree. So, as before, the same four node will be pruned. On the right side, you can see that we've switched the right two branches. We evaluated this branch and got a 4. So, we know that in the mid-node above, our value will be less than or equal to 4. However, the final node is a max node and we already have a 5 to compare that to. So, we'll always end up choosing this 5 in the max node above. That means that the right most branches are relevant and thus could be pruned. 

## Title 51 - Thad’s Asides 

Many problems in artificial intelligence are exponential in time or space, or both. In fact, NP hard problems are so common in AI that researchers joke half seriously that AI is the study of finding clever hacks to exponential problems. Often when a clever hacker is finally found, or when computers finally get fast enough to address that particular problem successfully, the world no longer thinks of the problem as one belonging to artificial intelligence. How many people think about the system that helps consumers choose plane flights as an AI? Or the system that helps determine when to deploy an airbag on an SUV? Yet at one point, these types of problems were considered to be part of AI. That idea leads me to another joke definition of AI. Artificial intelligence consists of all the NP hard problems that have not yet been solved. What matters to me are the problems that the field of AI tackles. On the way here, I was practicing this lecture with my taxi driver, Dilber, when I realized he was using AI while we were talking. To get me to Udacity headquarters, the navigation program on his smartphone had plotted a path that was better than the one I would have told him. And was communicating with him in a way that he was getting the information as he needed it. That is what is exciting about AI. You get a chance to work on everyday problems that can improve people's lives. 

## Title 52 - Solving 5x5 Isolation 

Let me introduce a special guest student from a previous class, Malcolm Haines. Hi, Malcolm. 
&gt;&gt; Hi, Thad. 
&gt;&gt; When working on the 5x5 isolation game in class, you believed you managed to search the end game. 
&gt;&gt; That's right. But when we talked about branching factor earlier, we said it would be impossible to search to endgame in a reasonable amount of time. So how did you do it? 
&gt;&gt; First, I implemented the minimax algorithm with alpha beta pruning. As you know, this reduces the size of the search space from b to the d, to b to the d over two, thus reducing it from approximately 8 to the 25th to 8 to the 12th. Next, I realized that some moves are equivalent. For example, let's look at player one's first move. [BLANK_AUDIO] If player one moves to 0, 0 it's the equivalent of moving to 4, 0. You can simply rotate the board 90 degrees and you have the same board state. Therefore, if you know the game tree for our first move of 0, 0, you know the game tree for our first move of 4, 0, which is the same as 4, 4 but also 0, 4. This is useful especially at the beginning of the game when the branching factor is high. For example, while player one has 25 possible moves, in reality, there are only six unique moves, as you can use symmetry about the horizontal and vertical axis. And about the diagonal axis, and of course, there's the center move. We've just shown player's one six unique first moves and their equivalents. A similar analysis is possible for any board state which is defined as a series of ord moves. Let's run through an example. In this example, o moved to 2.2. Now it's x's turn. x is considering possible moves and first evaluates a move to 2.1. x now knows the value of the board state, (2,2), (2,1). Now, let x consider a move to 2,3. x will check to see if the outcome of this move is already known. To do this x checks to see if he knows the value of the board state (2,2), (2,3). He does not. Next, x rotates the board 90 degrees and checks to see if he knows the value of the board state, (2,2), (1,2). He does not. Next, x board takes the board 180 degrees, and checks to see if he knows the value of the board state, (2,2), (2,1). Hah, it does. x returns the value of the board state (2,2), (2,1) and does not need to expand the game tree further. If x hadn't found a solution, he would have checked the board states created by rotating the board 270 degrees and flipping the board along its diagonal axis. Only then would x have needed to expand the game tree. 
&gt;&gt; So you found that symmetry really cut down on the number of nodes you had to expand? 
&gt;&gt; Only until about level 3 of the search tree. After that I gave up because symmetry was rare and the amount of effort needed to check for symmetry just wasn't worth it. 
&gt;&gt; So you're telling me that just those tricks are sufficient to search the endgame? 
&gt;&gt; Not quite. While alpha beta pruning in equivalent boards tremendously reduced the number of possible game states, they still weren't enough. Luckily, while searching for a good evaluation function, I discovered that I didn't need to always search to the end of the game tree. It turns out that I know the outcome as soon as there's a partition. 
&gt;&gt; Why is that? 
&gt;&gt; Well, a partition separates two players completely. Therefore, the player with the longest path wins. For example, In this game, player o moves next. But the outcome of this game is already known, because player eight has eight possible moves while player two only has six. 
&gt;&gt; Okay. So partitions, alpha-beta, and symmetry allows you to finish after you completed looking through all possible moves. Does someone always win if they play optimally? Yes Thad, it turns out that player 2 always wins. 
&gt;&gt; Well that's unexpected. But you mentioned you also found a good first move for player one, right? 
&gt;&gt; That's right. At first, in fact I thought player one was always going to win. This is because of reflection. 
&gt;&gt; Reflection? What do you mean? 
&gt;&gt; Reflection is just a 180-degree rotation of the opponent's move. In trying to solve the game, I discovered an interesting phenomenon. If player one's first move is to the center and player two's first move is one that player one can reflect, then player one can always win. All she has to do is reflect every move player two makes. Okay then, how does player two avoid losing? 
&gt;&gt; By moving to a location that player one can not reflect. There are eight such possible moves of the 24 available to player two. I leave it as a an exercise for the student to determine those eight moves. 
&gt;&gt; We're going to have a competition where the students are going to try make the best isolation players they can. Do you have any advice for them? 
&gt;&gt; I sure do. If you're player one, move to the center square. Then if possible, reflect player two's moves and you will win. But, it's better to be player two. In this case, create a good book of opening moves and hints. We've discussed the best first move if player one occupies the center square, and it turns out that in most cases if player one does not occupy the center square then player two should occupy the center square. Anyway, once you have a good book of opening moves, use your understanding of equivalent moves and hash tables to load and search your order book efficiently. Remember, you only have a limited amount of time to complete your move. After exhausting your book of opening moves, implement minimax then add iterative deepening, and finally alpha beta pruning. Finally, focus on your evaluation function. Now a good evaluation function is, well, that's up to you to decide. Good luck. 
&gt;&gt; Thank you, Malcolm. [BLANK_AUDIO] 

## Title 53 - 3-Player Games 

What about three player games like 3-player isolation? 
&gt;&gt; What's 3-player isolation? 
&gt;&gt; It's the same as normal isolation but with three players trying to be the last to move. 
&gt;&gt; So do the players form alliances against each other? 
&gt;&gt; They can, but there can only be one winner in the end. 
&gt;&gt; That could make the evaluation function difficult. 
&gt;&gt; Why don't we ignore that for now and just use #my_moves to make things simple? How would Minimax work? 
&gt;&gt; Well, for multiplayer games we don't use Minimax any more. Instead we evaluate the gameboard from the perspective of each player and propagate the values up the tree. 
&gt;&gt; How does that work? 
&gt;&gt; Let's imagine the 3-player isolation game tree where we search down to the level 3 of the tree. On the leftmost branch we evaluate the resulting game board from each of the player's perspectives. For player 1, the valuation function returns a 1. For player 2, it's a 2. And for player 3, the valuation is a 6. I guess we evaluate each of the board nodes at this level and then return triplets for each of them. 
&gt;&gt; Yep, and then we propagate the values of the tree. We first choose the max value at each of the level 2 branches from the player 3's perspective. The leftmost node, player 3 has a choice between 6 and 3. So of course, we choose 6. In the next branch from the right on that level, there is a choice between a 2 and a 1, so we choose 2. And the third branch from the left we choose between a 2 and a 1, so we choose the 2. And finally in the rightmost branch, we choose the 5. 
&gt;&gt; Okay, and for the next level up I guess we choose the maximum value from player 2's perspective. 
&gt;&gt; Yep. 
&gt;&gt; So on the left branch we choose the branch with the 2 and, on the right branch we chose the 5. 
&gt;&gt; Precisely, and at the top level we choose the maximum value from player 1's perspective. In this example, both options are equally good. So we choose the one on the left by default. This version of a game tree, which we call MAX N, can work for multiplayer games with any numbers of players. 
&gt;&gt; Let's try a quiz on this concept. 

## Title 54 - 3-Player Games Quiz 

Fill in the values of this three-player game tree, which alternates levels between players. The values of each node are in the order, player one score, player two score, and player three score. Each agent tries to maximize their own score, so will propagate up the values based on which agent's move it is. 

## Title 55 - 3-Player Games Quiz Solution 

Each agent tries to maximize its own score, so we propagate up the values based on which agent's move it is. We start with agent 3's level. Here agent 3 has a choice between a 1 and a 2, so it'll choose the 2. Then it has a 3 and a 4, so it'll choose the 4. Then it chooses between the 5 and the 6, so it'll take the 6. And then a 7 and a 8, so it'll take the 8. Next is a player 2 level. Player 2 is choosing between a 7 and a 5 ,so it's going to take the 7. And then a 3 and a 1, so it'll take the 3. Finally, it's player 1's turn. And player 1 will choose the 7 when compared to a 1. 

## Title 56 - 3-Player Alpha-Beta Pruning 

The next question is whether alpha beta can work with multiplayer games. 
&gt;&gt; Well according to a paper by Korsh, pruning can work as long as some of the values of the players and valuation functions, has an upper bound and each player's value has a lower bound. 
&gt;&gt; Well for our number of my moves the valuation function for isolation, zero is a natural lower bound. But what will make a good upper bound for the sum? 
&gt;&gt; If an evaluation function could estimate the number of total moves remaining for each player, it would work. Because the sum of the number of total moves should not exceed the number of empty squares on the isolation board. For example with this board, the limit should be 22. Or around seven per player. 
&gt;&gt; That's fine in theory. But the upper bound would change with each level of the game tree. Can't we simplify things for illustration purposes? 
&gt;&gt; Okay, we'll say that the sum of player scores can exceed ten. And to make things even easier, we'll say that the sum has to equal exactly ten. 
&gt;&gt; And that's going to allowing pruning? 
&gt;&gt; Yep, and we'll show an example that allows both immediate pruning and shallow pruning. 
&gt;&gt; Does that mean we can't do deep pruning like before? 
&gt;&gt; Unfortunately that's correct. While some pruning is possible, deep pruning like what alpha beta allowed before isn't possible. 
&gt;&gt; Okay, so show me an example of immediate pruning. Let's look at the left most bottom branch of the search tree. Since we know the maximum value is ten, there's no point in evaluating the next two branches to the right. They can return at most ten and we already have that option. We can safely ignore them and propagate the values up the tree. 
&gt;&gt; Great. And I guess we can put limits on what values will be at the top most node. However in this case it doesn't help. The value of player one could be anywhere from zero to ten. The other ones will be less than ten. But the middle branch would limit things a bit. And you will see an example of shallow pruning for the right branch. At the bottom level of the middle branch, player two will pick the left most option. Now we see player one will have a value greater than or equal to three. And since the sum has to be ten, each of the others are limited to values less than or equal to seven. 
&gt;&gt; But in the next branch over, player two will get a seven or better. Which means that player one will get a three or less. Since we already have a three, we can prune this last branch. Its values will not matter, because we're going to choose the option we already have. 
&gt;&gt; Now you see it. Let's have a quiz and work another example. 

## Title 57 - Probabilistic Games 

What if I want to make a computer player for a stochastic game, like Backgammon? 
&gt;&gt; Well, in Backgammon, your moves are limited each turn based on the roles of two dice. Since you can't know the result of the dice ahead of time, it would seem at first that you can't do a game-tree for it. In actuality, you can. And then use an algorithm called expect to max, to make best-choice decisions on your moves. To show how we handle probabilistic games, I've invented a variant of isolation I call sloppy isolation. 

## Title 58 - Sloppy Isolation 

To show how probabilistic games work, I've invented a version of isolation called Sloppy Isolation. In this game, players may not actually move where they intended. For example, if our player is trying to go to here, it only has an 80% chance of hitting the square intended. There's a 10% chance the player will undershoot its goal. And there's a 10% chance the computer player will overshoot its goal. But suppose the computer player is in a place where moving is very constrained. For example, here, if the computer player is moving to the right, it can't go beyond the border. In that case, there's 100% chance it'll land on the intended square. If the computer player is trying to move to the last square here, there's a 90% chance of it hitting its intended square and a 10% chance of it falling short. Similarly, if our computer player is just trying to move over by one, there's a 90% chance it will hit the square once and a 10% chance it will overshoot. 

## Title 59 - Sloppy Isolation Expectimax 

Let's use a simple six move isolation game board to illustrate the ideas. We'll add green circles to show probability nodes. It's O's turn and it has four possible moves. The first node we'll explore is the one where O tries to go to the left most top position, but if it tries, it'll only succeed 90% of the time. 
&gt;&gt; And 10% of the time it will end up at the position to its immediate left. 
&gt;&gt; Yep, its other moves are to try to go one move to the left, which it will do 90% of the time and 10% of the time, it'll overshoot. 
&gt;&gt; But if it attempts the other two positions it has no chance to overshoot or undershoot, so those will be easy 
&gt;&gt; Yep, but let's concentrate on the left most branch first. When it's X's turn, it'll have three possible moves. 
&gt;&gt; It can try to go immediately right with 90% chance of being successful and 10% chance of overshooting. 
&gt;&gt; Similarly, if it tries to go the whole way to the right, it will make it 90% of the time and undershoot 10% of the time. 
&gt;&gt; And if goes diagonally up and right it will hit it 100% of the time. 
&gt;&gt; So now we array to do some calculation. Using our standard number of my moves evaluation function. O has one move available to it in this game board. And two nodes available to it here. That means the expected value of this branch is 0.9 x 1 + 0.1 x 2 or 1.1. 
&gt;&gt; Then for the next node over we have a value of 2 for this node times 0.9 plus the value of the right node, which is 1, times 0.1. 
&gt;&gt; But notice we don't have to evaluate the right node. Since this level is a min level, as soon as we saw the 0.9 times 2 we had a 1.8. That's already bigger than the 1.1 we had on the left side, so we know we're never going to use this node. We don't have to evaluate this node at all. 
&gt;&gt; That's okay in this case, because we know the valuation function can't go below zero. But what if our valuation function could go negative? 
&gt;&gt; Well then we couldn't prune. In general, with EXPECTIMAX, you can only prune when you have known bounds on the values that will be returned by the EXPECTIMAX function. 
&gt;&gt; Okay, so here we'll actually choose the next branch because its value is 1 and its probability is also 1. 
&gt;&gt; So, expected value for this node is 1. Let's try the next sub tree over. 
&gt;&gt; This one is easy, looking at the bottom nodes, everything evaluates to 2, therefore all branches will evaluate to 2 and this node's value is also 2. 
&gt;&gt; Great, let's try the next sub tree. 
&gt;&gt; Here again, all the lower nodes are 2s. So the nodes value will be 2. 
&gt;&gt; The next one over is a bit more interesting, the left most bottom node has only one move remaining for O. 
&gt;&gt; And that's multiplied by a 0.9. Added to that is the 2 times the 0.1 in the node over in case X overshoots. That leads to a sum of 1.1. 
&gt;&gt; Looking at the next branch, if X tries to go the whole way to the right, we get a 2 times 0.9 or 1.8. We could prune this other branch now. 
&gt;&gt; But let's calculate it out for illustration purposes. 
&gt;&gt; Okay. Well, the evaluation function number of my moves which is turn one here. So 2 times 0.9 plus 1 times 0.1 would equal 1.9. 
&gt;&gt; But the last option is the best. It returns a 1 with a probability of 100%. 
&gt;&gt; So the value of this node would be 1. 
&gt;&gt; The next option is pretty easy to calculate. Everything comes up 2s. 
&gt;&gt; The one after that is also easy as all the moves are 100% accurate. Since it's a mid node we'll choose a 1. 
&gt;&gt; Great, now that we have all the values calculated, we can choose the branch with the highest value at the top of the max node. 
&gt;&gt; Looks like this branch wins. 
&gt;&gt; Yep, though it won't help O in this example game, O is going to lose unless X makes a bad choice or is unlucky. But if we know our player O is going to lose, why shouldn't we try to choose a branch that would still have the possibility for X to be unlucky or choose poorly? 
&gt;&gt; That is a good point. In fact, if we use this algorithm and search the end game, the algorithm might choose a branch where X could be unlucky and lose. Unfortunately, our simple number of my moves evaluation function is not really capturing that possibility. Searching deeper to end game should solve the problem. It'd be a good exercise to do, but now that we've covered everything, I'm excited to get back to the challenge question we started at the beginning of this section. 

## Title 60 - Expectimax Alpha-Beta Pruning 

At the beginning of this section, we gave a challenge question. Let's go over it carefully now. The leftmost branches values calculated by multiplying 0.1 x the minimum value which is 8 + 0.5 x the minimum value of the next branch which is 5 + 0.4 x 8. The sum of that is equal to 6.5. The bottom left node of the middle subtree returns a value of 0. 0.5 x 0 is still 0. We don't even need to evaluate the right branch of the middle tree now. The maximum we could get is a 0.5 x 10 which = 5. Since 5 is less than 6.5, there's no possible way for the right branch of the subtree to matter so we can ignore it. Note that if our nodes were ordered better, we could have had a 0 on the leftmost branch. Then, we could have ignored everything else. As we know that the value of this subtree is going to be 5 or lower. In other words, we would have known that, 0.5 x 0 or less + 0.5 times 10 or less is still going to be less than or equal to 5. For the right subtree, we have 0.1 x 3 which = 0.3. We still to get a 10 in the next branch or on some of the next branches so we get a 0.9 times 10 out of the rest of the subtree. So we just still continue because we're going to actually get a 9.3 in the end. Note that there's a lesson here. We should probably evaluate these subtrees with the highest probabilities first and the highest expected values first. However in this case, we continue with 0.5 x 9 or 4.5. So we have 0.3 + 4.5 so far which is = to 4.8. There's still a possibility that we can get a value better than 6.5 so we continue. The next branch yields a 2. At this point, we know that this subtree is going to have a value that is less than or equal to 0.1 x 3 + 0.5 x 9 + 0.4 x 2, which = 5.6. Thus, we are done. The last branch does not matter. Our player is going to choose the leftmost branch. Let's go to Shelly and one final quiz on the section. 

## Title 61 - Probabilistic Alpha-Beta Pruning 

What nodes can be pruned in this probabilistic game tree? Check the boxes for nodes or even full branches that could be pruned. Then find expected values of each remaining node. You can enter an exact number or a range, like less than or equal to 4. 

## Title 62 - Probabilistic Alpha-Beta Pruning Solution 

Here's the answer. We can prune these nodes and these branches. The final answer is 5.2. 