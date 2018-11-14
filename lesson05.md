# Lesson 5

## Title 01 - Introducing Sebastian Thrun  1
It is my great pleasure today to introduce Sebastian Thrun. For AI researchers, Sebastian is well-known for probabilistic robotics in autonomous vehicles. His group is the first to win the DARPA Grand Challenge on this topic. And he got even more fame from leading Google's self-driving cars. He's also a founder of Udacity and of Google X. Today, Sebastian's going to teach us about the fundamentals of probability. So Sebastian, why is probability important to AI researchers? &gt;&gt; It's one of the most fundamental concepts, and it has to do with the fact that we often don't know. So early artificial intelligence assumed you know everything, and they put all these words together. And these machines were pretty okay, but they couldn't learn anything. And it was only when people realized that there's an ability to learn that makes people so different that the machines became really, really powerful. And the fundamental basis for ignorance, for lack of knowledge, for uncertainty is probability. That's why probability matters for everything in artificial intelligence. &gt;&gt; So one of the things I really like about Sebastian's presentation of probability is how he goes from basics to dependency to Bayes' rule, and from there to Bayes' nets. It makes AI seem like an extension of probability. So as you're going through this section, make sure to really understand Bayes' rule because we'll be continuing to use it throughout the course.

## Title 02 - Coin Flip 
[Thrun] So let's talk about probabilities. Probabilities are the cornerstone of artificial intelligence. They are used to express uncertainty, and the management of uncertainty is really key to many, many things in AI such as machine learning and Bayes network inference and filtering and robotics and computer vision and so on. So I'm going to start with some very basic questions, and we're going to work our way up from there. Here is a coin. The coin can come up heads or tails, and my question is the following: Suppose the probability for heads is 0.5. What's the probability for it coming up tails?

## Title 03 - Coin Flip Solution 
[Thrun] So the right answer is a half, or 0.5, and the reason is the coin can only come up heads or tails. We know that it has to be either one. Therefore, the total probability of both coming up is 1. So if half of the probability is assigned to heads, then the other half is assigned to tail.

## Title 04 - Coin Flip 2 
[Thrun] Let me ask my next quiz. Suppose the probability of heads is a quarter, 0.25. What's the probability of tail?

## Title 05 - Coin Flip 2 Solution 
[Thrun] And the answer is 3/4. It's a loaded coin, and the reason is, well, each of them come up with a certain probability. The total of those is 1. The quarter is claimed by heads. Therefore, 3/4 remain for tail, which is the answer over here.

## Title 06 - Coin Flip 3 
[Thrun] Here's another quiz. What's the probability that the coin comes up heads, heads, heads, three times in a row, assuming that each one of those has a probability of a half and that these coin flips are independent?

## Title 07 - Coin Flip 3 Solution 
[Thrun] And the answer is 0.125. Each head has a probability of a half. We can multiply those probabilities because they are independent events, and that gives us 1 over 8 or 0.125.

## Title 08 - Coin Flip 4 
[Thrun] Now let's flip the coin 4 times, and let's call Xi the result of the i-th coin flip. So each Xi is going to be drawn from heads or tail. What's the probability that all 4 of those flips give us the same result, no matter what it is, assuming that each one of those has identically an equally distributed probability of coming up heads of the half?

## Title 09 - Coin Flip 4 Solution 
[Thrun] And the answer is, well, there's 2 ways that we can achieve this. One is the all heads and one is all tails. You already know that 4 times heads is 1/16, and we know that 4 times tail is also 1/16. These are completely independent events. The probability of either one occurring is 1/16 plus 1/16, which is 1/8, which is 0.125.

## Title 10 - Coin Flip 5 
[Thrun] So here's another one. What's the probability that within the set of X1, X2, X3, and X4 there are at least three heads?

## Title 11 - Coin Flip 5 Solution 
[Thrun] And the solution is let's look at different sequences in which head occurs at least 3 times. It could be head, head, head, head, in which it comes 4 times. It could be head, head, head, tail and so on, all the way to tail, head, head, head. There's 1, 2, 3, 4, 5 of those outcomes. Each of them has a 16th for probability, so it's 5 times a 16th, which is 0.3125.

## Title 12 - Dependence 
[Thrun] So let me ask you about dependence. Suppose we flip 2 coins. Our first coin is a fair coin, and we're going to denote the outcome by X1. So the chance of X1 coming up heads is half. But now we branch into picking a coin based on the first outcome. So if the first outcome was heads, you pick a coin whose probability of coming up heads is going to be 0.9. The way I word this is by conditional probability, probability of the second coin flip coming up heads provided that or given that X1, the first coin flip, was heads, is 0.9. The first coin flip might also come up tails, in which case I pick a very different coin. In this case I pick a coin which with 0.8 probability will once again give me tails, conditioned on the first coin flip coming up tails. So my question for you is, what's the probability of the second coin flip coming up heads?

## Title 13 - Dependence Solution 
[Thrun] The answer is 0.55. The way to compute this is by the theorem of total probability. Probability of X2 equals heads. There's 2 ways I can get to this outcome. One is via this path over here, and one is via this path over here. Let me just write both of them down. So first of all, it could be the probability of X2 equals heads given that and I will assume X1 was head already. Now I have to add the complementary event. Suppose X1 came up tails. Then I can ask the question, what is the probability that X2 comes up heads regardless, even though X1 was tails? Plugging in the numbers gives us the following. This one over here is 0.9 times a half. The probability of tails is 0.8, thereby my head probability becomes 1 minus 0.8, which is 0.2. Adding all of this together gives me 0.45 plus 0.1, which is exactly 0.55.

## Title 14 - What We Learned 
So, we actually just learned some interesting lessons. The probability of any random variable Y can be written as probability of Y given that some other random variable X assumes value i times probability of X equals i, sums over all possible outcomes i for the (inaudible) variable X. This is called total probability. The second thing we learned has to do with negation of probabilities. We found that probability of not X given Y is 1 minus probability of X given Y. Now, you might be tempted to say "What about the probability of X given not Y?" Is this the same as 1 minus probability of X given Y? And the answer is absolutely no. That's not the case. If you condition on something that has a certain probability value, you can take the event you're looking at and negate this, but you can never negate your conditional variable and assume these values add up to 1.

## Title 15 - Weather 
We assume there is sometimes sunny days and sometimes rainy days, and on day 1, which we're going to call D1, the probability of sunny is 0.9. And then let's assume that a sunny day follows a sunny day with 0.8 chance, and a rainy day follows a sunny day with--well--

## Title 16 - Weather Solution  
Well, the correct answer is 0.2, which is a negation of this event over here.

## Title 17 - Weather 2 
A sunny day follows a rainy day with 0.6 chance, and a rainy day follows a rainy day-- please give me your number.

## Title 18 - Weather 2 Solution 
0.4

## Title 19 - Weather 3 
So, what are the chances that D2 is sunny? Suppose the same dynamics apply from D2 to D3, so just replace D3 over here with D2s over there. That means the transition probabilities from one day to the next remain the same. Tell me, what's the probability that D3 is sunny?

## Title 20 - Weather 3 Solution 
So, the correct answer over here is 0.78, and over here it's 0.756. To get there, let's complete this one first. The probability of D2 = sunny. Well, we know there's a 0.9 chance it's sunny on D1, and then if it is sunny, we know it stays sunny with a 0.8 chance. So, we multiply these 2 things together, and we get 0.72. We know there's a 0.1 chance of it being rainy on day 1, which is the complement, but if it's rainy, we know it switches to sunny with 0.6 chance, so you multiply these 2 things, and you get 0.06. Adding those two up equals 0.78. Now, for the next day, we know our prior for sunny is 0.78. If it is sunny, it stays sunny with 0.8 probability. Multiplying these 2 things gives us 0.624. We know it's rainy with 0.2 chance, which is the complement of 0.78, but a 0.6 chance if it was (inaudible) sunny. But if you multiply those, 0.132. Adding those 2 things up gives us 0.756. So, to some extents, it's tedious to compute these values, but they can be perfectly computed, as shown here.

## Title 21 - Cancer 
Next example is a cancer example. Suppose there's a specific type of cancer which exists for 1% of the population. I'm going to write this as follows. You can probably tell me now what the probability of not having this cancer is.

## Title 22 - Cancer 2 
And yes, the answer is 0.99. Let's assume there's a test for this cancer, which gives us probabilistically an answer whether we have this cancer or not. So, let's say the probability of a test being positive, as indicated by this + sign, given that we have cancer, is 0.9. The probability of the test coming out negative if we have the cancer is--you name it.

## Title 23 - Cancer 3 
0.1, which is the difference between 1 and 0.9. Let's assume the probability of the test coming out positive given that we don't have this cancer is 0.2. In other words, the probability of the test correctly saying we don't have the cancer if we're cancer free is 0.8. Now, ultimately, I'd like to know what's the probability they have this cancer given they just received a single, positive test? Before I do this, please help me filling out some other probabilities that are actually important. Specifically, the joint probabilities. The probability of a positive test and having cancer. The probability of a negative test and having cancer, and this is not conditional anymore. It's now a joint probability. So, please give me those 4 values over here.

## Title 24 - Cancer 3 Solution 
And here the correct answer is 0.009, which is the product of your prior, 0.01, times the conditional, 0.9. Over here we get 0.001, the probability of our prior cancer times 0.1. Over here we get 0.198, the probability of not having cancer is 0.99 times still getting a positive reading, which is 0.2. And finally, we get 0.792, which is the probability of this guy over here, and this guy over here.

## Title 25 - Cancer 4 
Now, our next quiz, I want you to fill in the probability of the cancer given that we just received a positive test.

## Title 26 - Cancer 4 Solution 
And the correct answer is 0.043. So, even though I received a positive test, my probability of having cancer is just 4.3%, which is not very much given that the test itself is quite sensitive. It really gives me a 0.8 chance of getting a negative result if I don't have cancer. It gives me a 0.9 chance of detecting cancer given that I have cancer. Now, what comes (inaudible) small? Well, let's just put all the cases together. You already know that we received a positive test. Therefore, this entry over here, and this entry over here are relevant. Now, the chance of having a positive test and having cancer is 0.009. Well, I might--when I receive a positive test--have cancer or not cancer, so we will just normalize by these 2 possible causes for the positive test, which is 0.009 + 0.198. We know both these 2 things together gets 0.009 over 0.207, which is approximately 0.043. Now, the interesting thing in this equation is that the chances of having seen a positive test result in the absence of cancers are still much, much higher than the chance of seeing a positive result in the presence of cancer, and that's because our prior for cancer is so small in the population that it's just very unlikely to have cancer. So, the additional information of a positive test only erased my posterior probability to 0.043.

## Title 27 - Bayes Rule 
So, we've just learned about what's probably the most important piece of math for this class in statistics called Bayes Rule. It was invented by Reverend Thomas Bayes, who was a British mathematician and a Presbyterian minister in the 18th century. Bayes Rule is usually stated as follows: P of A given B where B is the evidence and A is the variable we care about is P of B given A times P of A over P of B. This expression is called the likelihood. This is called the prior, and this is called marginal likelihood. The expression over here is called the posterior. The interesting thing here is the way the probabilities are reworded. Say we have evidence B. We know about B, but we really care about the variable A. So, for example, B is a test result. We don't care about the test result as much as we care about the fact whether we have cancer or not. This diagnostic reasoning--which is from evidence to its causes-- is turned upside down by Bayes Rule into a causal reasoning, which is given--hypothetically, if we knew the cause, what would be the probability of the evidence we just observed. But to correct for this inversion, we have to multiply by the prior of the cause to be the case in the first place, in this case, having cancer or not, and divide it by the probability of the evidence, P(B), which often is expanded using the theorem of total probability as follows. The probability of B is a sum over all probabilities of B conditional on A, lower caps a, times the probability of A equals lower caps a. This is total probability as we already encountered it. So, let's apply this to the cancer case and say we really care about whether you have cancer, which is our cause, conditioned on the evidence that is the result of this hidden cause, in this case, a positive test result. Let's just plug in the numbers. Our likelihood is the probability of seeing a positive test result given that you have cancer multiplied by the prior probability of having cancer over the probability of the positive test result, and that is--according to the tables we looked at before-- 0.9 times a prior of 0.01 over-- now we're going to expand this right over here according to total probability which gives us 0.9 times 0.01. That's the probability of + given that we do have cancer. So, the probability of + given that we don't have cancer is 0.2, but the prior here is 0.99. So, if we plug in the numbers we know about, we get 0.009 over 0.009 + 0.198. That is approximately 0.0434, which is the number we saw before. 
