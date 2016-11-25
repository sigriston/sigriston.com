---
layout:     post
title:      "The Airplane Seating Problem"
date:       2016-11-24 22:59:00 -0200
categories: statistics
---
This is an interesting problem I bumped into the other day. What's so
interesting about it is that, while it requires only basic knowledge of
probability to solve, it is far from trivial and the answer may be quite
surprising! Here's the problem:

> A line of 100 passengers is waiting to board a 100-seat airplane. However,
> the first passenger in line forgot his designated seat and will sit in a
> random seat. The following passengers will sit in their assigned seats unless
> their seat is already taken, in which case they will sit in a random
> available seat.
>
> What is the probability the 100th passenger will manage to sit in her
> designated seat?

Tricky, huh? Try to work out the answer to that in your head and then come back
here later to find out if you got it right!

Okay, let's try and solve this now. For starters, let's investigate what
happens in a much smaller plane, of only 2 seats, and with only 2 passengers.
We'll use the notation $$n = 2$$ for this case, meaning the number of both
seats and passengers is 2. Now we'll introduce the notation $$P_n$$ which is
the probability the $$n$$-th passenger will find her seat available. So right
now let's work on finding out $$P_2$$, the solution to the 2-seat problem.

Turns out, $$P_2$$ is really easy: either the first passenger sat in his
assigned seat, in which case the second passenger will find her seat available;
or he didn't, in which case the second passenger's seat will be occupied. We
know from basic prob that this is basically a coin toss, with 50% chance for
each.  So our answer is that passenger 2 has 50% chance to find her seat
available.  Using our notation:

$$P_2 = \frac{1}{2}$$

Now let's work with a 3-seat, 3-passenger plane. The first passenger can sit in
any of three seats, at random: his own, passenger 2's, and passenger 3's.  The
chance for each is exactly one third.

In the first case, we know that passenger 3 will find her seat unoccupied, and
in the third case she won't. Let's introduce some notation to represent this:
$$P_{n|x}$$, meaning the probability that the $$n$$-th passenger will find her
seat available given that the first passenger sat in passenger $$x$$'s seat.
So, for the 3-seat case we found out that:

$$P_{3|1} = 1 \qquad P_{3|3} = 0$$

What about $$P_{3|2}$$? Let's think about it. If the first passenger sits in
passenger 2's seat, now passenger 2 will have to sit in a random seat: either
passenger 1's or passenger 3's. Now, haven't we seen that one before? It looks
exactly like the 2-seat problem, in which passenger 3 will have a 50% chance to
find her seat available.

$$P_{3|2} = \frac{1}{2}$$

This is not a coincidence: when passenger 1 sat in passenger 2's seat, we can
say that passenger 2's assigned seat is now passenger 1's, and solve this
exactly like the 2-seat problem. In notation:

$$P_{3|2} = P_2 = \frac{1}{2}$$

Now, remembering that the chance of passenger 1 to sit in any given seat was
exactly one third, so it follows that:

$$P_3 = \frac{1}{3} P_{3|1} + \frac{1}{3} P_{3|2} + \frac{1}{3} P_{3|3}$$

Substituting the probabilities we already calculated:

$$P_3 = \frac{1}{3} \times 1 + \frac{1}{3} \times \frac{1}{2} + \frac{1}{3}
\times 0 = \frac{1}{2}$$

Hmmmmmmm... so again the probability is 50%. Interesting. Let's investigate the
4-seat problem to see if a pattern emerges, then. Starting with the basics:

$$P_4 = \frac{1}{4} P_{4|1} + \frac{1}{4} P_{4|2} + \frac{1}{4} P_{4|3} +
\frac{1}{4} P_{4|4}$$

Just like with 3 seats, we know the trivial cases. If passenger 1 sits in his
assigned seat, passenger 4 will manage to do so; and if passenger 1 sits in
passenger 4's seat right away, then she'll never manage to do it:

$$P_{4|1} = 1 \qquad P_{4|4} = 0$$

For the other two cases, we can do exactly like we did before, and "downgrade"
our 4-seat problem to a smaller problem. If passenger 1 sits in passenger 2's
seat, we can say that passenger 2's assigned seat is now passenger 1's, and
treat it like a 3-seat problem!

$$P_{4|2} = P_3 = \frac{1}{2}$$

And in case he sits in passenger 3's seat, then we can reassign passenger 3 to
passenger 1's seat and treat it like a 2-seat problem:

$$P_{4|3} = P_2 = \frac{1}{2}$$

Putting it all together:

$$P_4 = \frac{1}{4} \times 1 + \frac{1}{4} \times \frac{1}{2} + \frac{1}{4}
\times \frac{1}{2} + \frac{1}{4} \times 0 = \frac{1}{2}$$

Huh! 50% again! And, if we think back to how we solved $$P_{3|2}$$, $$P_{4|2}$$
and $$P_{4|3}$$, a pattern **did** emerge: if the first passenger takes a seat
that is neither his nor the last passenger's, the passenger who lost his seat
can swap assignment with passenger 1 and we can treat this like a smaller
problem.  Let's try to put that into notation.

Considering we're dealing with a $$k$$-seat problem, and passenger 1 just sat
in passenger $$x$$'s seat, then we can say that they swap seat assignments and
we'll treat this as a problem of size $$(k - x + 1)$$:

$$P_{k|x} = P_{k - x + 1}$$

Putting it all together to solve $$P_k$$:

$$P_k = \frac{1}{k} P_{k|1} + \frac{1}{k} P_{k|2} + \frac{1}{k} P_{k|3} +
\ldots + \frac{1}{k} P_{k|k}$$

$$P_k = \frac{1}{k} \times 1 + \sum_{i = 2}^{k - 1} \frac{1}{k} P_{k - i
+ 1} + \frac{1}{k} \times 0$$

$$P_k = \frac{1}{k} + \frac{1}{k} \sum_{i = 1}^{k - 2} P_{k - i}$$

Now if we consider that all problems smaller than $$k$$ had probability of one
half:

$$P_2 = P_3 = P_4 = \ldots = P_{k - 1} = \frac{1}{2}$$

Then we can solve $$P_k$$ by substituting all the smaller problem probabilities
by one half, as follows:

$$P_k = \frac{1}{k} + \frac{1}{k} \sum_{i = 1}^{k - 2} P_{k - i}$$

$$P_k = \frac{1}{k} + \frac{1}{k} \times \frac{k - 2}{2}$$

$$P_k = \frac{2}{2k} + \frac{k - 2}{2k} = \frac{k}{2k} = \frac{1}{2}$$

Considering we already know that $$P_2$$ is one half, we can solve $$P_k$$ for
any $$k > 2$$, and the result will always be one half. Thus, we have proven by
induction that:

$$P_n = \frac{1}{2}, \forall n \geq 2$$

That is the surprising (at least for me) part of the problem: no matter how
many seats (either 2, 200 or 2 million), the answer always will be 50%.

Evidently, this means that $$P_{100}$$ of our original problem will be 50%, the
same as with any other $$n$$, so we can state the following answer:

> The probability that the 100th passenger will find her seat free is 50%.
