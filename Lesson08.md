# Title 01 - Pattern Recognition through Time Intro

Thad, we've covered a lot of different machine learning algorithms. But what about situations where we have time series? 
&gt;&gt;you mean like speech recognition? 
&gt;&gt;yeah, I know you've worked in sign language recognition and handwriting recognition. They seem to be similar problems. 
&gt;&gt;yep, they are. Pattern recognition through time is one of my specialties. And it's where the AI community has made a lot of progress. When you think about it, pattern recognition through time should be easier than situations whereyou have just one measurement. 
&gt;&gt;you mean like how face recognition is easier when you have a video of a person, as opposed to a single photo? if one particular frame of the video is badly lit or the person is looking away the next frame might be better. 
&gt;&gt; Precisely. But in this section, we are going to focus on situations where there's a language-like structure to the problem. in other words, there's some fundamental unit, like a phoning in speech, or letter in handwriting, that's combined with other units to make bigger constructs like words. Which are then combined in even larger structures, like sentences. 
&gt;&gt; So beside speech, sign language and handwriting, what other problems have the same sort of structure? 
&gt;&gt; Well, in my opinion, most human activities through time have a language-like structure. Playing basketball, driving a car, or even just vacuuming the floor, all seem to have familiar units of movement, combinations of those movements and source statistical grammar. i believe this area of AI is one of the most promising for future research. i'm excited to share my knowledge of the subject. 
&gt;&gt; And then we're going to cover dynamic time warp and hidden Markov models in this section. Where should we start? 
&gt;&gt; Well, let's start with a challenge question on HMMs that gives a preview of what we're going to be learning. 


# Title 02 - HMM Challenge Question

Assume we have two left or right three state HMMs, HMM A and HMM B. Their topologies and transition probabilities are provided below. Note that each state has only one Gaussian. The mean is shown in the figure and all the Gaussian have a standard deviation of 1. We want to use Model A and Model B for optical character recognition. An image with these models is included in the instructor notes, soyou can refer to it as you do this quiz. Let the horizontal axis be t and the vertical axis be y in these graphs. Given a character, for example the L in this figure, the input will be a sequence of v values indexed by time from left to right. v is computed by counting the number of black squares not the y values in each column. For example, for this L, the values will be 5,1,1,1 and 1. For these figures here, locate which model most likely produce the data, either model A or B? As usual, we do not expect that you will know how to sort this problem. There's more preview of a lesson to come.



# Title 03 - HMM Challenge Question Solution

We could compute the likelihood that the input features for each character were generated from model A as well as from Model B, and then choose the model with the higher likelihood. However, the task is much easier because all of the states from all of the models have the same transition probabilities. What makes models A and B different are these output probabilities. Model A expects a high value at first, a low value in the middle and a high value at the end. Model B expects the opposite, a low value at the beginning, a high value in the middle and a low value at the end. Looking at the letter H, it has a lot of vertical ink at the first value, a little in the middle and a lot at the end. Some it matches model A better. The character I's features go low, high, low. And so model B wins. T has the same pattern, so model B wins again. This last one is a little more complicated. The square has the same constant, high value throughout the time. However, here, model B is most dislike the data because it has two states that expect low values. Whereas model A has two states that expect high values. So it must be model A. [BLANK_AUDIO]



# Title 04 - Dolphin Whistles

Let's start with a problem I'm currently exploring, dolphin communication. Here's a spectrogram of a dolphin whistle. in actuality, dolphins have several types of vocalizations, including burst pulses and echolocation, but whistles are the easiest to see in a spectrogram. 
&gt;&gt; First, let's talk about what a spectrogram is. On this spectrogram, the x-axis is time, and the y-axis is frequency. The brightness of the pixels indicate the amount of power in that frequency band. 
&gt;&gt; Since we are recording in the ocean there's a lot of noise in the lower frequencies. a lot of boat and wave noise. 
&gt;&gt; [SOUND]

&gt;&gt; However, the whistles of the Atlantic spotted dolphin tend to range from 5kHz-17kHz. 
&gt;&gt; [SOUND]

&gt;&gt; These whistles can be heard up to 3 miles away under water. 
&gt;&gt; Why do they whistle? 

&gt;&gt; Probably for a lot of reasons. But one thing we do know is that dolphins have signature whistles that act much like our names. if a dolphin enters a new area and feels like company, It whistles his signature whistle, which helps its friends know where it is and how to find it. 

&gt;&gt; So what are we seeing in this spectrogram? 
&gt;&gt; To me it looks like the same whistle repeated twice. The whistle has two parts. Our task is to be able to recognize classes of whistles, so that our marine mammal researchers can automatically annotate their database of recordings. 
&gt;&gt; Well, I see one problem already. The second time the whistle is repeated, it seems a little more drawn out.



# Title 05 - Problems Matching Dolphin Whistles

That problem is going to be the focus for this lesson. We need to be able to handle classes of signals, where each example may be warped in time a bit differently. 
&gt;&gt; Okay, well, for features, can we just use the whistle frequencies through time? 
&gt;&gt; Normally, that would be a good idea, but in practice, the dolphin's often raise or lower the basic frequency of the whistle. What seems to matter is the pattern of relative rises and falls of the whistle through time. 
&gt;&gt; In that case, let's use delta frequency. Here, instead of 5, 14, 10, let's use 9, -4, -3, 3, and 4.



# Title 06 - Warping Time

That will help our recognizer handle the whistles no matter what frequency they start at. Next let's work on the time warping. 
&gt;&gt; Just to be clear you're talking about the problem where I could draw out your name saying Fad or say your name quickly like Fad. 
&gt;&gt;yep, that's precisely the problem we have. But with dolphin whistles, the problem is easy to see in the spectrogram. Basically we need to match the features that make the whistle distinct no matter if they are drawn out or produced quickly.



# Title 07 - Euclidean Distance Not Sufficient

You always say do the simple thing first and add intelligence only if necessary. What happens if we just do
Euclidean distance here? 
&gt;&gt; Let's try it and find out. Here are some reasonable delta frequency numbers for the top and bottom graphs. 
&gt;&gt; The top time series has 21 samples, but the bottom one only has 12. 
&gt;&gt; How do we do a Euclidean distance? 
&gt;&gt; A simple thing to do is pad the bottom one with 0s. 
&gt;&gt; That's reasonable. Other options would be to fill in with the average value or the last value, but any of these options will work for our example. 
&gt;&gt; Now we can figure out the Euclidean distance by simply doing the square root of the squares of the differences. Let's see, 0 minus 0 squared is 0, 
&gt;&gt; And 2 minus 5 squared is 9. And 3 minus 5 squared is 4. 
&gt;&gt; And so on. Doing the math, we get the square root of 170, which is approximately 13. 
&gt;&gt;yep, now let's try using dynamic time warping through the same problem.



# Title 08 - dynamic Time Warping

Okay, so all dynamic time warping is doing is trying to align the samples between two whistles we're comparing so that they best match up. 
&gt;&gt;yep, let's line up the two signals we are trying to compare on the x and y-axes. That will allow us to more easily see how we are matching the samples. 
&gt;&gt; Okay, let's put the shorter one on the y-axis for convenience. 
&gt;&gt; A match without any time warping would be a straight diagonal going from the lower left-hand corner to the upper right. But for the types of singles we're comparing, that would be rare. 
&gt;&gt; We know we have to start with the first sample in each signal. in this case, they're both 0. 
&gt;&gt; For the next sample in the x-axis, we have another 0. is it better to stay matched to the 0 in the shorter sequence or transition to the 5 in the next sample? 
&gt;&gt; Well, there will be less difference if we stay with the 0. 
&gt;&gt; So we'll draw a horizontal line here indicating that we're matching the first two 0's on the x-axis with the first 0 on the y-axis. 
&gt;&gt; For the next sample we have a 2. That still matches with the 0 better than the 5. 
&gt;&gt; So we continue the horizontal line. 
&gt;&gt; But then we go to 3 on the x-axis which matches the 5 better. 
&gt;&gt; So we transition up one on the y-axis to indicate that. 
&gt;&gt; We're going to have several How do we match them to the 5's on they-axis? 
&gt;&gt; Well, we're going to need a transition to the 2 and 
So let's make our transitions so as to keep us close to the diagonal line as possible. 
&gt;&gt; I get it. We're trying to match the values as closely as possible to minimize the error. And if the values otherwise tie, we try to keep to the diagonal. 
&gt;&gt; Correct, the 3's will match the 5's pretty well. And by the time we get to the 2, the 3 matches that even better. When we get to the 2 and the 1 on the x-axis, it'll match very well with the 2 and 0 up here. And the 0 and
-1 match the 0 pretty well. 
&gt;&gt; Then we can match the -2, -3, -3, and -1 sequence on the x-axis to the 3,
-3 on the y-axis. 
&gt;&gt; And the rest of the long sequence, matches the matches the 1, The last 1 has to match the last 
&gt;&gt; Now we can calculate the euclidean distance as before, but this time we had better matches. 
&gt;&gt;yep, writing it out, we have the distance being the square root 0 minus 0 squared which is 0. Plus 0 minus 0 squared again, which is again 0.  
&gt;&gt; I think we get the idea. 
&gt;&gt; Sorry, i always have to see these things written out to really understand them. After a lot of scribbling, we get a distance of the square root of 34, which is approximately equal to 6. 
&gt;&gt; That is less than half of the distances we calculated before when we just padded the shorter sequence with 0's. 
&gt;&gt; Which shows the power of dynamic time warping. For situations where we know the signals we are comparing, we'll have some sections that are faster and some sections that are slower compared to each other. DTW is a very useful tool. 
&gt;&gt; I see a problem, though. 
&gt;&gt; What's that?



# Title 09 - Sakoe Chiba Bounds

Let's suppose we have two signals that really are not that similar. dynamic time warping could allow them to match much better than they should. Look at this example where we use a different shorter signal along with the same long one as in the last example. 
&gt;&gt; Okay. 
&gt;&gt; While I have to start by matching the 0 and the 3, i can match the first half of the x-axis sequence to the 3 so that I can get to the negative section of the sequence to get a better match. Then I'm in a positive section of both sequences again. 
&gt;&gt;yep, that can be a problem. in this case, the distance is poor because of all those numbers the first part of the sequence, matching the 3. But the severe amount of warping we're allowing means that the distance is probably smaller than we'd like. 
&gt;&gt; One way we could force a more reasonable matching is to bound how much we're allowed to deviate from the diagonal. 
&gt;&gt;yep, we can use Sakoe Chiba Bounds to force more reasonable matches. 
&gt;&gt; Sakoe Chiba, now you're just making things up. 
&gt;&gt; No, no, really. it says we won't allow matches outside the limits placed by these diagonal lines. 
&gt;&gt; That will cause the matches to be worse leading to a bigger distance. 
&gt;&gt; Which is what we want. 
&gt;&gt; How do we calculate these bounds? 
&gt;&gt; Often empirically, we set different bounds and use cross validation to make sure they are reasonable. 
&gt;&gt; Well, what if some sections of the signal should have different bounds than others? 
&gt;&gt; Like on our example, this section here might have a lot of variance being shorter along, but the hump may have less variability and shape. 
&gt;&gt; And perhaps the last section can then have more variability again. Basically there are three sections of the signal and each can have a different amount of allowed warping. 
&gt;&gt; We could try to have different Sakoe Chiba Bounds for each section of the signal. 
&gt;&gt; True, but that seems complicated to train. And I knowyou prefer hidden markup models for such problems. Why don'tyou introduce them? 
&gt;&gt; Okay,you know they're my favorite technique.



# Title 10 - Hidden Markov Models

Hidden Markov models, or
HMMs as they're known to their friends, are a useful tool for looking at pattern recognition through time. They're similar to other Markov models, in that they have states that represent a set of observed phenomena, and a set of transitions that describe how we can move from one state to another. We'll be considering first order Markov models which only depend on the state immediately preceding them and not a history of states. Hidden Markov models are a variant that are used for recognition of many types of signals that have a language-like structure. 
&gt;&gt; What's hidden about them? 
&gt;&gt; With HMM, we don't necessarily know which state matches which physical event. instead, each state can yield certain outputs. We observe the output over time and determine a sequence of states, based on how likely they were to produce that output. 
&gt;&gt; I see, since HMMs and codes sequences over time, we could use them for recognizing things like speech, handwriting or gesture. 
&gt;&gt; Exactly, and HMM researchers have spent decades figuring out tricks like state tiling, context, stochastic grammars and boosting to improve performance. in this lecture, we will start with how to decode an HMM, where we calculate which model best fits the sequence of data.



# Title 11 - HMM Representation

We should probably go over how to represent an HMM. 
&gt;&gt; Right, the Russell and Norvig book draws them like a Markov chain and adds an output node for each state. in this representation, which is common in the machine learning community, eachxi represents a frame of data.xo is the beginning state which is useful for keeping data.x1 represents the first time frame t equals 1. E1 represents the output at that time.x2 is the next time frame and
E2 is the output at that time. And so on, until we get to the end of the sequence. The HMM states are implicit in this representation. However, I find it easier to think of HMMs in terms of their states. So I'm going to deviate from the book and use a representation that is more specific to HMMs for this discussion. To get things started let's imagine we have a signal through time that looks like this graph. At t equals 0, its value is -2. By t equals 10 it is at -1, by t equals 15, it's at 0. And at t equals 35 it's at 1, at t equals 38 it's at 2. 
&gt;&gt; So it looks like the graph is made up of 4 different parts. 
&gt;&gt; That's right, we're going to use a 4 state HMM to represent it. We are trying to design a model that could have generated this data. in this case we are going to use a left-to-right HMM, meaning that we never transition back to a previous state once we've left it. These loops are called self transitions, which indicates that the model can stay in the same state for several timeframes. 
&gt;&gt; Next we need to figure out the emission probabilities. 
&gt;&gt; I like to call them alpha probabilities, but it's just a different name for them. All it means is which values that are allowable while we are in a given state. 
&gt;&gt; It's a little bit more subtle than that, right? Since the output distributions are densities, these are not really probabilities. 
&gt;&gt; But it's a convenient fiction for our discussion, so i'm going to stay with it. in this case, creating the output probabilities is easy. The first part of the graph is from -2 to -1. 
&gt;&gt; And all values are equally represented, so we can just use a box car distribution. 
&gt;&gt; Great, for state two the upper distribution is going to be a box car from -1 to 0. 
&gt;&gt; In state 3, we have a box starting from 0 to 1, and state 4 is from 1 to 2. 
&gt;&gt; Now we need to figure out the values for the transition probabilities. Let's look at the first part of the graph again. We spend about 10 time frames in the first part of the graph before we transition to the second part of the graph. So we want to assign an appropriate probability here where we escape state 1. 
&gt;&gt; Well, that seems of one off, let's assign a probability of 0.1. That way on average we expect to stay in that state for 10 frames. 
&gt;&gt; Since all the probabilities out of a state have to sum to 1, and we only have the self-transistion probability left, we know that it has to be 0.9. 
&gt;&gt; And here's a little trick, if we want to know the number of time frames we expect to stay in a given state. We can just use the formula, probability, to figure it out. 
&gt;&gt; Thanks, for some reason, I keep forgetting to use that trick when I'm working on more complex HMMs where there are a lot of transitions out of the state. 
&gt;&gt; We can continue this process for each of the states, state 2 has 5 frames. So we'll make this output probability one-fifth or 0.2 and this one four-fifth or 0.8. State 3 has 20 frames, so this arrow gets a one-twentieth, or a 0.05 and this is 0.95. And the last transition out of the model is one-third, or 0.33, with the cell's transition being 0.67. 
&gt;&gt; Hold it, I just realized something,you said I could figure out how many frames I expect to be in the state by using the formula, probability. But if I set the probability to 0, i get 1 over 1-0, which equals 1. That means I'll get at least 
&gt;&gt; But that's fine, we output as soon as enter the state from the previous state, and then we transition to the next state. 
&gt;&gt; Hm, Okay, that explains something else to me. A lot of manuals and toolkits post dummy state at the beginning of each model. i guess to explicitly represent entering the first state. Actually that leads me to another point. When I'm being more formal, i just put the arrow at the first state. if we can enter the model at several different positions with equal probability, I put these arrows at each potential point of entry. if the entry points have different probabilities, i'd write them at these arrows. 
&gt;&gt; There are more details we will cover later, but we have shown what we need to create an HMM by inspection to represent a given signal

&gt;&gt; In practice, we expect to have lots of examples of a signal we want to model. And we'll have to create a model that can accommodate all of the different examples, balancing both generalization and overfitting. But that will come later, for now, we will assume that we can create an HMM by inspection. in fact, the technique is robust enough that inspection will work for a first pass at a problem. 
&gt;&gt; How about we move on to a real problem? 
&gt;&gt; Sounds like a good idea.



# Title 12 - Sign Language Recognition

We will use sign language recognition as our first application of HMMs. For example, let's consider the signs I and we and create HMMs for each of them. Here's I. [BLANK_AUDIO] We is a little different. [BLANK_AUDIO] Let's focus on the I gesture. We'll use delta y as our first feature here. 
&gt;&gt; Wait a second. Why don't we use delta x? it looks like it would be easier to differentiate the two words that way. While delta y is pretty similar for the both of them. 
&gt;&gt; That's right. But I want to show exactly how powerful HMMs can be. So I have purposefully chosen a bad feature. We'll actually still be able to tell the difference between the two words by the difference in timing. Let's go through the process and see how that works. 
&gt;&gt; Okay, but sometimes it's hard to visualize the derivative of a signal. Let's have a quiz first to make sure we understand what you're talking about.



# Title 13 - Delta-y Quiz

Here are several plots of y versus t. Given these plots, match each of the y versus t plots with their derivative plots, delta y versus t.



# Title 14 - Delta-y Quiz Solution

Here's the answer. [BLANK_AUDIO]



# Title 15 - HMM_ _I_

Okay, I've made an HMM for sign language word, I. 
&gt;&gt; Great, how did you pick those states? 
&gt;&gt; Well, the gesture seemed like it had three separate motions. So, I made each of those their own state and chose the transition probabilities based on the timing. 
&gt;&gt; We can take a look at the observed delta y here to check to see if the model is reasonable. First, the arm comes up towards the chest, and then pauses for a moment and then goes back down again. We can see that we spend a lot of time in states one and three, so there's a high probability on their self loops. State two is short so we've given it a smaller probability. 
&gt;&gt; Right, then we can fill in the output probability distribution similarly. For example in state two, we have a higher probability of getting a delta y of zero. We use Goshens to model the upper probabilities here since they have nice properties.



# Title 16 - HMM_ _We_

Great, now here's the HMM I created for the gesture, we. [BLANK_AUDIO] 
&gt;&gt; Hold on. i would have used four states here. Why did you only use three? 
&gt;&gt; Well it was mostly to simplify the problem for our purposes. Note that the middle section varies a little bit more in delta y than with the middle state of I. And we spend more time in the middle state as well. 
&gt;&gt; I see, it looks like the transition probabilities have changed to reflect this, right? 
&gt;&gt; Correct, we spend longer in that state, so we have a higher probability in the self loop. Also, we have more variation in the delta y in this gesture, which leads to a wider, and shorter, output distribution. 
&gt;&gt; This seems like a good time for a short quiz.



# Title 17 - I vs We Quiz

What property of the observed sequences of delta_ys can help tell the difference between the two gestures? Probability distributions in respect to starting states, probability distributions in middle states, likely time spent in middle states, or none of the above? Select all answers that could apply.



# Title 18 - I vs We Quiz Solution

Here's the answer. it could be probability distributions in middle states, as well as likely time spent in middle states



# Title 19 - Viterbi Trellis_ _I_

So how do we actually go about performing recognition with the models we just created? 
&gt;&gt; Suppose we have a set of observations, O, that represent the samples in an example we want to recognize. We'll create something called the
Viterbi Trellis to see how likely each model generated the samples in O. The one that gives us the highest probability will be considered the proper match. However, we don't know the exact sequence of states that created the output. Those are hidden, so we'll need to go through all the possibilities. 
&gt;&gt; That sounds like it would be long, but maybe we can simplify it as we go. 
&gt;&gt; Okay, here's the sequence of delta ys we collected in the example we want to recognize as I or we. 
&gt;&gt; So our overall goal is to find P of O given lambda I or the probability of our observation sequence given our model of I. 
&gt;&gt; Correct. Here's how we start the trellis. We have one row for each state in the model and we want to look at which state we're in at each time step. 
&gt;&gt; Well at the beginning we have to start in state 1, correct? 
&gt;&gt; That's right. 
&gt;&gt; Okay, then at t equal to 2, we can stay in state 1 or transition to state 2, but we can't reach state 3 yet. Looks like we've already eliminated some of this trellis. 
&gt;&gt;yes, the transition probabilities in these models really limit where you can be at any given time. Perhaps we can have a quiz on that idea.



# Title 20 - _I_ Transitions Quiz

What state could we be in at t equals to 7? What about t equals 6? Check the boxes on each possible transition.



# Title 21 - _I_ Transitions Quiz Solution

Here's the answer. We know we need to end at state 3. Before that, we could only have been in state 2 or state 3.



# Title 22 - Viterbi Trellis_ _I_ (continued)

in the middle of the trellis it looks like we have many more options. We could really be in any of the three states. 
&gt;&gt; That's true. The next thing we need to do is to add transition probabilities. We can pull those directly from our model, lambda I. 
&gt;&gt; Okay, I have added the transition probabilities, that seems simple enough. But how do we go from this to an overall probability? 
&gt;&gt; Here's where our observation sequence comes in. Let's look at time step 1. We saw a delta y of 3, but we know we can only be in state 1 at that time. By looking at the output distribution at state 1, we can see how probable it is to get a 3 from it. Normally, we would have a real distribution for these outputs. And we could find the actual probability for the output. But for illustration purposes, i'll pick some reasonable numbers. As long as we're consistent the example should work. i'll call the probability that the Gaussian in the first state generated a delta y of 3 to be 0.5. So let's write that into this node. 
&gt;&gt; I see. So then for time 2 we have an output of 7. 
And 7 is also two away from 5. So it will be a probability of 0.5 here as well. 
&gt;&gt; That works. Now we also need to consider state 2. But the probability of getting a seven there is very small, nearly zero, but not quite. So let's say, 10 to the -7. 
&gt;&gt; Why don't we pause for a quiz to make sure this process makes sense?



# Title 23 - Nodes for _I_

For each of the nodes that t equals to that is closest to the output probability. Note that we have filled in the probabilities up to this point to make it easier foryou to see what's going on.



# Title 24 - Nodes for _I_ Solution

Here's the answer for t equal to 5. We've also gone ahead and filled in the rest of the trellis.



# Title 25 - Viterbi Path

We can follow this process to fill out all the nodes in our charts. 
&gt;&gt; Now, we need to look for the most likely path. Each time frame will modify the transition probability times the output probability. Note, to the most likely path may not be the greedy path, in other words. The highest expected value with each transition, may not necessarily lead to the highest valued overall path. in this case the maximum path since, be this one. 
&gt;&gt; Okay, so let's consider the transition from time one to time two. We can stay in state 1 or move to state 2. What should we use to compare the two options? 
&gt;&gt; Expected value of staying in state 1 is 0.8 times 0.5,yet going from state 1 to state The greedy algorithm would expect to stay in state 1 since that value is bigger. 
&gt;&gt; Okay, I see, how does Viterbi account for the overall path? 
&gt;&gt; We need to keep track of the probability of each possible sequence of the trellis. There are only two nodes in each sequence so far, so the two probabilities are 1 x 0.5 x 
&gt;&gt; Or we could've gone down 1 times 0.5 times 0.2 times 10 to the negative 7th which is equal to times to negative 8. 
&gt;&gt; It looks like these numbers could get really small. 
&gt;&gt;yes, in practice we should use log space to calculate these probabilities. Otherwise we run out of precision in the way the OS represents numbers. To keep things simple, let's keep going. 
&gt;&gt; Okay, next at t equal to 3, our expected values from state 1 are .8 times .6 and
.2 times 10 to the negative 5. it looks like we should multiply the previous result, .2 by our new expected values. 
&gt;&gt; Right

&gt;&gt; We also have to account for the path that went through state two earlier.That one will become ten to the negative eight times zero point five time ten to the negative five or ten time zero point five time ten to the four. 
&gt;&gt; Free state, we going to keep the path with maximum value, for state one is going to be this number. For state two, it's going to be this number, for state three, it'll be this number. We'll continue this process keeping track of the maximum path to get to each state at each time through the trailers. At the end, we choose the most likely path. For this trellis, it turns out to be this one. The final answer for the probability of the observation given our model is about 0.00035. We can then compare this probability to the corresponding result from the trellis for we.



# Title 26 - _We__ Transitions Quiz

Now it's your turn. Can you tell us the most probable path through the trellis for the model of we? We'll start small. Check the boxes between states to indicate which state transitions can occur. For example, check this box to show that state two can transition to state three.



# Title 27 - _We__ Transitions Quiz Solution

Here is the answer, the transitions though the trellis looks like this. Note that this actually the same as the transitions for the model of I.



# Title 28 - _We__ Transition Probabilities Quiz

Now fill out the transition probabilities for each arrow shown on the trellis based on the model we created earlier.



# Title 29 - _We__ Transition Probabilities Quiz Solution - lang_en_vs50.srt

Here is the answer. Note that the main difference between I and We is the transitions for state two.



# Title 30 - _We__ Output Probabilities Quiz

in the last quiz, you looked at the transition probabilities. Now, let's consider the output probabilities. We filled out some of the probabilities to get you started. Choose from these answers and fill out the remaining nodes in the trellis.



# Title 31 - _We__ Output Probabilities Quiz Solution

Here's the answer. [BLANK_AUDIO]



# Title 32 - _We__ Viterbi Path

Finally, we need to determine the most likely sequence through the trellis. Check the boxes to indicate the best path and then fill out the probability of that path here.



# Title 33 - _We__ Viterbi Path Solution

Here's the most likely path through the trellis. Notice that it's very similar to the path for i, but the probability is much smaller.



# Title 34 - Which Gesture is Recognized_

So it looks like it's a lot more probable that the model for i generated this data. 
&gt;&gt;yep. The main difference between the values for the models producing this observation sequence has to do with the middle state. Remember that we used delta y even though it is a relatively bad feature for distinguishing I from we. The output probability Gaussian for the middle state of both models have a mean value of zero. However, with I, the expected probability of getting an actual zero at the middle state is much higher than with we, a 0.9 versus a 0.7. 
&gt;&gt; Also with the gesture I, we spend much less time in the middle state than with We. The transition probabilities for the middle states reflect this difference. With I, our transition from the middle state to the last state is 0.5 but with We it was 0.3. This example shows how well HMMs can distinguish between two gestures, even with relatively poor features. 
&gt;&gt; What is great about HMMs, is that the difference in values for even relatively weak features, accumulates through time. The longer the gesture is, the easier it is to recognize. Perhaps we can experiment with that idea in a quiz.



# Title 35 - New Observation Sequence for _I_

Let's look at a new observation sequence. We've replaced the middle 0 observation with a new sequence -1 0 and 1. Given these probabilities, can you tell us the probability of this observation sequence, given the model for I?



# Title 36 - New Observation Sequence for _I_ Solution

Here's the answer. By multiplying all the transition and output probabilities, along with the curvy path and the new trellis, we get the resulting probability for i 1.42x 10 to the negative 5th.



# Title 37 - New Observation Sequence for _We_

Now lets do the same thing for We. We have the same observation sequence as the previous quiz, where the middle zero was replaced with the sequence negative one, zero, and one. We've given you new probabilities for
We. So go ahead and tell us the probability of this observation sequence given our model for We.



# Title 38 - New Observation Sequence for _We_ Solution

Here's the resulting probability for
We, 2.91 x 10 to the -5. Note that this answer is higher than what we got for the model of i. indicating that this observation sequence probably came from a We gesture. This is a different result from what we saw previously. Showing how the additional time spent in the middle state maps better to our model for We.



# Title 39 - HMM Training

When we started this lesson, we create our models by inspection, however, most of the time we want to train using the data itself. When using HMMs for gesture recognition, i like to have at least 12 examples for each gesture I'm trying to recognize, five examples at a minimum. 
&gt;&gt; For illustration purposes let's just use three examples of data for gesture eye. Even though it's not the best feature we are going to continue using deltay for training, the first example is the longest,it has 16 data points. 
&gt;&gt; And I see the next one is really short was only five data points. The last one has 14 timeframes. Let's use a three state left to right topology again for Himercap model. For each of this examples, how are we going to figure out which data goes with which state? 
&gt;&gt; Let's assume all three states have about an equal number of frames. i've divided the examples roughly in the thirds and drawn a boundary between each third. 
&gt;&gt; On average are example Step with main about 4 data points first date, for example. 
&gt;&gt;yup, which means a transition probability out of this stage is going to be 1/4. 
&gt;&gt; And since the transitions probability have to sum to one for each date. The self transitions are 3/4. 
&gt;&gt; Now that we've fixed the transition probabilities, let's calculate the output probabilities. 
&gt;&gt; For simplicity, let's use a single galceon again. For the first eight, we assign the 1 the 2 and 10 from the second example and the 1 3 7 8 7 to this first state. The average of that is approximately 5.4. The standard deviation is, hold on there, let me get out my copy of Octave, about 3.1. Let me draw this under the first state as its output probability. 
&gt;&gt; Since you're being handy, can you do the other states too? We can calculate the mean value of the second state. it's around negative 0.6 with a standard deviation of 3.1. The last state has a mean of minus 5, standard deviation of about 2.6. 
&gt;&gt; Let's try those out probabilities as well. 
&gt;&gt; Now we're going to iterate in the process very similar to expectation maximization. 
&gt;&gt; So basically we reset the transition probabilities and calculated the output probabilities based on the assumption that the transition probabilities are correct. Then we're going to assume that the output probabilities are correct and go back and adjust the transition probabilities and so on until we converge. 
&gt;&gt; So basically our next step is to see if moving these boundaries on these examples between the states will lead to a better explanation of the data. Basically, we are looking to see if we get Gaussian of less variance. 
&gt;&gt; Well, that could be interesting, let's look at the first boundary in the first example. Cleary the 5 is closer to the mean of 5.4 so the boundary should move to the right. 
&gt;&gt; But the two might to better than a second state. 
&gt;&gt; How do you display that? 
&gt;&gt; Well two is 3.4 the mean of the first state. Which is 3.4 over 3.1 standard deviations away. in other words two is a little more than one standard deviation away from the first state. 
&gt;&gt; Okay. 
&gt;&gt; But the mean for the second state is -0.6 which make the two only 2.6 units away from the mean of the second state. Or 2.6 over 3.1 standard deviations away. 
&gt;&gt; Which is less than one standard deviation away. So two should stay in the second state. 
&gt;&gt; For now. 
&gt;&gt; What do you mean, for now? 
&gt;&gt; By the time we finish this iteration, the mean and standard deviations for the states will change and two might move in the first state in the next round. 
&gt;&gt; Okay, but how many of the boundaries change in this iteration? 
&gt;&gt; Let's look. i don't think the -1 is going to move from the second stage. 
&gt;&gt; But in the second example does -7 much closer to the main third stage -5. 
&gt;&gt;yup, lets move the boundary. 
&gt;&gt; Shouldn't we update the main in variant. 
&gt;&gt; Its better to face looking at rest of the boundaries first. Last example looks like we have a lot of things we can move. 
&gt;&gt; Right, the three should probably move from the second state to the first. 
&gt;&gt; And certainly the minus five should go from the second to the third. 
&gt;&gt; How about the negative three in the second state. i have to calculate that one out quick. it's 2.4/3.1 standard deviations from the second state. And two over 2.6 standard deviations from the mean of the third state. it is slightly closer to the mean of the third state, though not by much. 
&gt;&gt; Great, let's update the boundaries again. 
&gt;&gt; Wow, that's going to cause a big change in our numbers. 
&gt;&gt;yep, let's recalculate everything. 
&gt;&gt; On average, we now have one, two, three, four, five, six, seven, eight, nine, ten, 11, 12, 13, 14. expect in the first date. So our transition probability here is going to be one over 4.7. Or approximately 0.21 making the self-loop 0.79. 
&gt;&gt; The second state has 7 over Transition probably there will be .43 and .57. 
&gt;&gt; And the last state has 14 over 3 time frames on average or again approximate .21 transition problem here and .79 problem here. 
&gt;&gt; Okay, so now we need to update the output probability numbers. 
&gt;&gt; I was afraid you are going to say that. Okay, clinging at octave again, i get a mean of 5.2 and a standard deviation of 3.0 for the output probability for the first state. 
&gt;&gt; Come on,you're the one with the computer. What's the rest? 
&gt;&gt; Fine, state two is easy. it has a mean of 0 and a standard deviation of 1. 
&gt;&gt; Do you do that on purpose? 
&gt;&gt; What? 
&gt;&gt; Make the example so the numbers would come out nice? 
&gt;&gt; I wish I was that smart. 
&gt;&gt; I'm so disillusioned. Okay, what are the values for state three? 
&gt;&gt; State three's mean is -5 and standard deviation about 2.5. 
&gt;&gt; Great, now we can do our next iteration. 
&gt;&gt;you mean, we gotta do it again? 
&gt;&gt; Don't be a cry baby, it's almost done. 
&gt;&gt; Okay, well, as I predicted, this 2 is now closer to the first date on this iteration in the second state

&gt;&gt; Well, I guess you win that one. it's just a little over one standard deviation from the mean of the first state. And two standard deviations from the mean of the second state. 
&gt;&gt; But the -1 still stays in the second state. 
&gt;&gt;yup, in both places. 
&gt;&gt; So let's calculate again. 
&gt;&gt; Now state one's mean goes to five. State two's mean goes to slightly negative. And state three's mean stays the same. 
&gt;&gt; I think we've converged. Let me take a look.yeah, in the next iteration none of the boundaries are going to move nor are the ALPA probably going to change. 
&gt;&gt; Does that mean we've successfully trained HMM for I? 
&gt;&gt; Not yet. Technically speaking, what we've done is use Viterbi alignment to initialize the values for each state of our HMM. in other words, for each time frame in each of our examples we've assigned a state for that time frame and calculated the resulting averages of all values assigned to each state. 
&gt;&gt; Along with the average amount of time we expect to stay in each state. 
&gt;&gt; Correct.



# Title 40 - Baum Welch

So what's next? 
&gt;&gt; A process called
Baum Welch re-estimation. 
&gt;&gt; That's like
Expectation-maximization again, right? 
&gt;&gt; Correct. 
&gt;&gt; But how does it differ from what we just did? 
&gt;&gt; It's very similar, but with Baum Welch, every sample of the data contributes to every state proportionally to the probability of that frame of data being in that state. So you're saying that even though this its values still effects state two and three. 
&gt;&gt; But only a little bit because it's likelihood of belonging to states two and three is pretty low. 
&gt;&gt; But for data like this 2 here, it has a high likelihood of being in state one or state two. 
&gt;&gt; And thus, has a bigger influence on the final output probabilities for those states than for state three. 
&gt;&gt; Okay, that doesn't sound so bad in theory. But it sounds hard to calculate because you have to weight everything appropriately. 
&gt;&gt; It's actually not that bad. We can use the forward-backward algorithm to help keep track of the calculations. And it's worth the effort, because the final model often gives better results than our initial estimate. 
&gt;&gt; Now that we've given the intuition for what is going on, there are several good writeup's on the details of Baum Welch. Tutorial is a classic reference, and
Thad's master thesis has an explanation that continues with the sign language example. Or just looking at the code to a popular
HMM toolkit like HTK can be useful, too. in any case, using one of these toolkits and watching the means and variants change as the re-estimation process iterates will help build further intuition, and we highly suggest it.



# Title 41 - Multidemensional Output Probabilities

Now that we've shown how HMMs work, let's provide some more tips on how to improve them. 
&gt;&gt; Okay, in our example of using HMMs to distinguish between the signs i versus we, we used delta y as a feature. But in reality delta x would be a better feature. 
&gt;&gt; That's true, but for other signs delta y would be a good feature. Another good feature is the size of the hand which helps us get some information as to if the hand is coming towards the camera or away. 
&gt;&gt; Another good feature might be the angle the hand makes to the horizontal. 
&gt;&gt; So sign language is two handed, we should have these features both to the right and left hands. Now that we have eight features, we are tracking for time frame, how do we integrate it into our models? 
&gt;&gt; Actually, it's pretty easy. We just add more dimensions for the output probabilities. All our training and recognition work like before. We just have to calculate multi-dimensional distances instead of using just one dimension. 
&gt;&gt;you're right. it gets hard to show on our graphs here but it's easy enough to code.



# Title 42 - Using a Mixture of Gaussians

What if our output probabilities aren't Gaussian? 
&gt;&gt; Well according to the central limit theorem, we should get Gaussians if enough factors are affecting the data. 
&gt;&gt; But in practice sometimes the output probabilities really are not Gaussian. it is not hard for them to be bimodal. 
&gt;&gt;you mean like this. 
&gt;&gt;yep. 
&gt;&gt; Well then we can use two Gaussians to model it. 
&gt;&gt; Basically using the mixture of Gaussians technique. 
&gt;&gt;yep, in theory we can model anything with enough Gaussians. Look at this box card distribution. 
&gt;&gt; The more Gaussians we use the closer we will model it, but how may should we use? 
&gt;&gt; That really depends on your data. Visualization really helps here. in practice I tend to limit my mixtures to two or three Gaussians, otherwise it tends to over fit.



# Title 43 - HMM Topologies

Next, let's talk about increasing the size of our vocabulary. 
&gt;&gt; Okay, I've selected some signs we can use to start making phrases. But we're going to have to choose topologies for each of them. 
&gt;&gt; Well, we've already chosen topologies for I and we. What sign is next on your list? 
&gt;&gt; Well, let's add, want [BLANK_AUDIO]. How many states does that look like to you? 
&gt;&gt; Well it has the onset where you bringyour hands up, the actual motion for the sign itself. And then you're droppingyour hands again. i would say three states. 
&gt;&gt; Well how about for, need? [BLANK_AUDIO] 
&gt;&gt; That looks similar, so i'd also use three states. 
&gt;&gt; Okay here's a trickier one. This is an example for the sign for table. [BLANK_AUDIO] But, hold on, that was actually a poor example. There's a better example for table. 
&gt;&gt; So your hands go up and down twice? That seems like four states in this case. 
&gt;&gt;yep, but to make thing even more interesting, here's another example. [BLANK_AUDIO] 
&gt;&gt; Hold on, that time you went up and down three times. How many times is allowed? 
&gt;&gt; Well, generally you'll see the motion repeated twice. But if somebody's thinking hard about what to say, they might repeat the motions, stalling for time. it's sort of like an English speaker saying or. 
&gt;&gt; Okay, so how can we deal with that in an HMM? 
&gt;&gt; Well, I didn't say we always had to go from left to right with an HMM 
&gt;&gt; Okay. So we can actually make a loop in the
HMM, say from the third to the second state, so that the motions can be repeated any number of times. 
&gt;&gt;yep, you got the idea. in practice, we can try lots of different typologies, use cross-validation with randomly selected, training-independent test sets to settle on the best one. 
&gt;&gt; Another example of your, try the simple thing first, strategy? 
&gt;&gt; Exactly, but in this case, lets try the simple typology first and only add states when necessary. Let's look at another issue. Here's an example for cat, [BLANK_AUDIO] And here's another example for cat [BLANK_AUDIO]. 
&gt;&gt; One is one-handed, while the other is two-handed. i don't think we're going to solve that with a simple typology trick. 
&gt;&gt; We could try just a normal three-state HMM, then use a mixture of Gaussians to help handle the probabilities we get. 
&gt;&gt; But I think it makes more sense just to recognize these as two different signs. 
&gt;&gt; Well, how so? 
&gt;&gt; Well we could train a model for the first variation of the sign. We'll call it cat one. And we'll train a separate model for the second variation and call it cat two. And then we recognize either cat one or cat two, we'll just have our recognizer output, cat. 
&gt;&gt;yep, that should solve the problem. While this approach adds more gestures to our vocabulary, it is often the approach that results in highest accuracy, provided we have enough examples to train all the variations.



# Title 44 - Phrase Level Recognition - lang_en_vs50.srt

Now that we have topologies for our six signs, let's talk about phrase level sign language recognition. We have eight phrases we want to recognize. 
&gt;&gt; Actually,you mean 7 signs and 12 phrases. 
&gt;&gt; Since we have two variants of cat we are recognizing, expanding all the possibilities leads to 12 phrases. 
&gt;&gt; Good point. To keep things simple, let's use a strict grammar. We will always have a pronoun followed by a verb followed by a noun. And we'll only recognize one phrase at a time. 
&gt;&gt; So let's expand that out again to be explicit. 
&gt;&gt; Great. Now comes the messy part. 
&gt;&gt; Let's assume we have data from thad's signing that we want to recognize. Here he is actually signing I need cab. Really, that's the phrase you came up with for us to use? 
&gt;&gt; Everyone should have a cat. 
&gt;&gt; Meow. 
&gt;&gt; Okay, anyway suppose looking at the delta y feature through time gives us this string of observations. 
&gt;&gt; Which is about 20 samples through time. 
&gt;&gt; We're going to do recognition the same way as before, but with a much bigger trellis. 
&gt;&gt; We have 22 states to worry about. Three for each of I, WE, NEED, WANT,
CAT1 and CAT2 and four for TABLE. 
&gt;&gt; So where can we be when t is equal to one? 
&gt;&gt; Well, the grammar says we have to start with I or WE. So we have to be in the first state, of either these two pronouns. 
&gt;&gt; Where can we be when t equals to two? Well, we could stand the current state. We could transition into the second state of I or we. 
&gt;&gt; How about t equal to three? 
&gt;&gt; It's still easy. We can be in states one through three for I or for we. We haven't had a chance to transition out the first sign yet. 
&gt;&gt; But what happens with t equal to four? Once we get to t equals four, we could, at least in theory, have transitioned to either the verbs want or need. We are beginning to see the complexity, that will soon overtake us. 
&gt;&gt; That's right, but it's not too bad yet. At t equal to five, we have more opportunities where we can transition to need or want. But we still can't get to the nouns yet. 
&gt;&gt; At t equals six, we're still with the pronouns and verbs. 
&gt;&gt; But, at t equals seven, we could, possibly, transition to TABLE, or CAT1, or CAT2. 
&gt;&gt; At t equals eight, we could get into the second state of the noun signs, and a t equals nine, we can actually transition into the third state of the noun signs

&gt;&gt; At t equals a ten we have the first time we can transition back from state three to state two in table. This loop back complicates our trellis a little bit. 
&gt;&gt; But it's not too bad. We just now have this back and forth structure. 
&gt;&gt; And here's the full trails for after we taking care of all the data from the example phrase. 
&gt;&gt; Wow. And this is the simplest example we could come up with. But note here, we couldn't actually reach all this states in the end. We have to get back to the bottom states of one of the three nouns. 
&gt;&gt;yup, this trail also shows how HMM is going to take up a lot of memory. 
&gt;&gt; Imagine if we had a vocabulary that included all of the six thousand signs in the American Sign Language, or even worse yet, all the variations of those signs that give the ASL it's expressiveness and speed. 
&gt;&gt; Not to mention what would happen if we were using a real grammar? Which brings up an interesting point. Almost all of language recognizers will have problems keeping the full trellis and memory. 
&gt;&gt; And keeping all the paths updated will also be a problem. 
&gt;&gt; How will we deal with this problem? 
&gt;&gt; We'll use the caustic beam search.



# Title 45 - Stochastic Beam Search

Stochastic beam search, did we see that before? 
&gt;&gt; Not in detail. So far we've been doing something like a breadth first search, expanding each possible path in each time step. But now we want to prune some of those paths. 
&gt;&gt; Well some of those paths are going to get a low probability pretty quickly. For example, staying in the first state of the model for i until t equals 10 seems improbable. We could just drop some of those low probability paths. 
&gt;&gt;yup, that's the idea of the beam search. But we don't want to get rid of all the low-probability paths. it is possible that there is a bad match in the beginning of the phrase that becomes a good match later on. For example, the signer might hesitate or accidentally start with the wrong sign before changing it. 
&gt;&gt; Like someone stuttering or having a false start in a spoken language. 
&gt;&gt; Precisely. 
&gt;&gt; In that case, let's keep the paths randomly in proportion of their probability. 
&gt;&gt; Here are some examples of high probability paths through the trellis marked in red. And some low probability paths through the trellis marked in blue. 
&gt;&gt; In that case, let's keep the paths randomly in proportion with their probability. 
&gt;&gt;yep, the idea has some similarity to the fitness function we talk about with genetic algorithms. Or to the randomness in simulated knee length. in practice, it works very well. 
&gt;&gt; Randomness seems to be useful in a lot of AI. 
&gt;&gt; It's a principle I practice in my daily life. 
&gt;&gt; What? 
&gt;&gt; Precisely. 
&gt;&gt; Okay, let's get back to the real topic.



# Title 46 - Context Training

OK. Now let's talk about another trick. When we moved from recognizing isolated signs to recognizing phrases of signs, the combination of movements looks very different. 
&gt;&gt; For example, when Thad signed NEED in isolation, his hands started from a rest position and finished in the rest position. When he signs NEED in the context of I NEED CAT, the first part of NEED runs into the last part of I, and the last part of NEED runs into the first part of CAT. His hands are no longer moving so much. 
&gt;&gt; That's right, the signs before and after a given sign can significantly affect how it looks. 
&gt;&gt; So instead of recognizing the three state model for need, we're going to concatenate a combined six state model of I NEED, and NEED CAT. 
&gt;&gt; In practice, we often have lots of examples of phrases of sign and not individual signs to train on. in my original paper on recognizing phrases of ASL, I had 40 signs in the vocabulary, but the only training data that I had was 500 phrases, that each contained five signs. 
&gt;&gt; In this case, let's suppose we have lots of examples of our three sign phrase. in our original example on training in
HMM in data, we assume that the data for an isolated version of the sign for i was evenly divided among its three states. We then calculated the output probabilities given that assumption, adjusted the boundaries and transition probabilities, and iterated until convergence. 
&gt;&gt; We're going to do the same thing for our first step here, but this time we will assume that the data is evenly divided between each sign, and then we'll divide the data for each state in each sign. 
&gt;&gt; And we iterate the same way we did before, adjusting the boundaries on each state and each sine until we converge. 
&gt;&gt; Now's when things get interesting. After we've converged everything for each sign, we're going to go back and find every place where I NEED occurs. Notice that there will be a lot fewer of those than there are examples of NEED. 
&gt;&gt; But let's assume that there are enough examples. 
&gt;&gt; Okay. Well we are going to cut out the data we think belongs to I NEED, and train the combined six state model on it. 
&gt;&gt; How does that help? 
&gt;&gt; Well the output probabilities at the boundary between I and NEED, here and here, as well as the transition probabilities in that region, will be tuned to better represent I NEED than the general case which would include we need. in speech the effect of one phoneme affecting the adjacent phoneme is called coarticulation, and this method of modeling is called context training. 
&gt;&gt; So I see we're going to do the same thing for NEED CAT1. 
&gt;&gt;yep, and for every other two sign combination. NEED-CAT2, WANT-CAT1, WANT-CAT2, i-WANT, WE-NEED, and WE-WANT. We are going to iterate using
Baum-Welch, using these larger contexts from embedded training, until we converge again. 
&gt;&gt; Why not use three sign contexts, or even more when the phrases are complex enough? 
&gt;&gt; If we have enough data that's not a bad idea, because the benefits are actually pretty large. For recognition tasks, where there's a language structure, we expect context training to divide our error rate in half.



# Title 47 - Statistical Grammar

Statistical grammars can help us even more. in our example up to this point, we used a simple grammar, a pronoun, verb and noun. And that placed a strong limit of were we started and ended in our Viterbi trellis. 
&gt;&gt; But in real life, language is not so well segmented. instead, we can record large amounts of language and affirm in the fraction of times need follows I, versus want following I, or want following we. 
&gt;&gt; We can then use these probabilities to help bias our recognition based on the expected distribution of the co-occurrence of these signs. in practice, using a statistical grammar divides the error rate by another factor of 4. We started with a fundamental error rate of E. When we used context training, we divide in half. And now with statistical grammars, we're dividing it by a factor of 4 again. So, basically, we end up with our error rate divided by 8. This trick is one of the secret weapons in my research. i look for problems that might have a language-like structure and then apply these techniques to get my error rate down to something reasonable.



# Title 48 - State Tying

Another trick is State Tying. 
&gt;&gt;you mean combining training for states with the states within the model are closed? 
&gt;&gt;yup. Let's look at our models for i and we again. in the case where we are recognizing isolated signs, the initial movement of the right hand going to the chest is very similar in both models. instead of having an I state one and a we state one, we're just going to have an initial state and define the I and we models such that they both include it. That way when we entrain the HMM we have twice as much data to entrain the initial state. 
&gt;&gt; And we can do the same thing with a final state, since the hand going back down to rest looks much the same in the isolated models for I and we. 
&gt;&gt; We have to be careful with this trick, because State Tying gets more complicated when we start to worry about context training. For our new table and we want CAT2, the only states that should be tied are the first state for I and we and the last state for table and cat. 
&gt;&gt; And maybe not even the last states for table and CAT2, if we're using more features than just deltay, the end of table and CAT2 could be very different. 
&gt;&gt; Good point, in practice I often just look for states that seem to have close means and variances during training and then determine if tying them looks logical given the motion I expect. Again it's a situation where some visualization of the data and iteration can help us improve our results.



# Title 49 - Segmentally Boosted HMMs

in your past work on gesture recognition, how many dimensions have you used foryour output probabilities? 
&gt;&gt; Up to hundreds. At one point, we are creating appearance models of the hand, using a similarity metric of how closely the current hand looked like different visual models of the hand as features for the HMM. However, the problem was there was a lot of noise in that the models that didn't match well were giving relatively random results. Causing many of the dimensions to be meaningless unless the hand at that particular time matched well. 
&gt;&gt; So all those dimensions were actually hurting you because they were all very noisy most of the time? 
&gt;&gt; Correct, however we can use boosting to help us weight the feature vector to combat this problem. This technique is called segmentally boosted HMMs. One of my students, did hi PhD dissertation on the idea. And his results were often For some datasets, we got up to 70% improvement. 
&gt;&gt; How does it work? 
&gt;&gt; First, we align and train the HMMs as normal. Next, we use that training to align the data that belongs to each date as best we can. We examine each state in each model iteratively. We boost by asking which features help us most to differentiate the data for our chosen state versus the rest of the states. We then weight the dimensions appropriately in that HMM. This trick combines some of the advantages of the discriminative models with generative methods. 
&gt;&gt; Is it in a toolkit somewhere? 
&gt;&gt; Not yet. But I'm thinking about adding it to HTK and our Georgia Tech Gesture Toolkit. Personally, I think it can be pretty powerful but it's not widely known yet.



# Title 50 - Using HMMs to Generate Data

One last thing I'd like to cover is why the normal HMM formulation is not good for generating data. in the early days of speech recognition, there was the hope that we could use the same HMMs we use to recognize speech, to also generate it. it turned out not to be such a good idea. Even though later advancements led to good results. 
&gt;&gt; Maybe an example is the best way to show the issue. 
&gt;&gt;you're right. Let's take this same HMM we had at the beginning of the lesson. 
&gt;&gt; The one which modeled the stray lines? 
&gt;&gt;yep, now with just looking at the HMM, not the example data, give me ten numbers that could be generated by the first state. 
&gt;&gt; Okay, -1, -1, -2, -1.5, -1, -1.25, -1.75, and -2. 
&gt;&gt; Now some numbers for the second state. 
&gt;&gt; 0, -0.5, -0.25, -0.75 and -1. 
&gt;&gt; Same thing for the third state. 
&gt;&gt; 1, 0, 1, 1, 0, 1, 1, 1, 0, 1, 0, 

&gt;&gt; And finally the last state. 
&gt;&gt; 1.5, 1.75 and 1. 
&gt;&gt; Plotting these numbers looks nothing like the original example because the output distributions have no idea of continuity. 
&gt;&gt; How would we fix that? 
&gt;&gt; We could use a lot of states to try to force a better ordering of the output, but that would lead to over-fitting. A better idea is to model the state transitions with an HMM. But use a different process while in each state that is more aware of the context to generate the final data. There are several methods that are possible. But the most sophisticated work I know of is by speech synthesis researchers. For example, Google has some good documents of how they are combining HMMs with deep belief networks to get better results.