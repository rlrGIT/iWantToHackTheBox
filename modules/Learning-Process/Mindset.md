## Learning Process : Mindset

March 29th, 2024

### Way of Thinking

- Principle of Abstraction (How you want people to think of something; how you consistently frame something)
- Principle of Correspondence
- Principle of Data Type Completeness

Evolve as a pen tester by tacking the following actions: find, choose, adapt, and learn. You should be able to think about why you are choosing the methodology you are when solving problems.

### E.g. Solve the following equation and explain why you did it

```
20 + ? + ? = 65535
20 + (-20) + 65535 = 65535       // create zero, make the math trivial
```

There are infinitely many solutions, and some are more difficult to find than others. In the naive approach, we could try to solve the equation by subtracting 20 from both sides, and use any combination of pairs that summed to 65515. But instead of doing that extra step of math, I thought I would just add a negative number, creating a zero, which would allow me to more easily choose a solution. 

### Think Outside the Box

```
Why didn't you think to add more digits or replace the given arithmetic operations?
```

(Note to self) Not 3 mins in and I've already been big brained.

### Why didn't you consider changing operations? Or adding more digits? 

I assumed that there were rules. I saw this equation, and I accepted the format that was presented to me, and I did not ask questions. Instead, I followed the basic rules that I had in my mind about how these kinds of equations were supposed to work. I think, I half replaced the operations by using a negative number, but I think what the author is trying to get at here is changing the problem like this:

```
20 + ? + ? = 65535
==> 65535 = 66535 or 20 = 65535 - 65515
```

Note that the above ways are not necessarily the most efficient, but explain the goal of the exercise. I think the goal here is to challenge the format of the problem you are being fed, and look for avenues of attack that break the abstraction that is presented to you. Here, we think of the idea of an equation, but do we KNOW that we HAVE to treat this as an equation? If that abstraction is broken, what can we do to achieve our goal? At the end of the day, we're just looking at text, but we impart meaning to it from our experience. This is deep, I think I am going to enjoy this course.

- Thinking outside the box helps us cross imaginary boundaries and thus access options that were not available to use before

### Occam's Razor

```
The most straightforward theory is preferable to all others of several sufficient possible explanations for the same state of facts. I.e. The simplest explaination is always the most probable.
```

- Ask "why is this happening?", and start to solve from the most probable reasoning. This helps limit options, enables us to start tackling the issue.
- This is hard to do in practice, because you must distinguishing between individual mechanisms and the overall concept of what is happening. Individual steps on their own, without direction of how they are applied, mean very little. 
- It is not always the case that the simplest solution is the most probable, but this about defining a process that enables us to work with the information we can get, acknowledging that it is always imperfect. As our image of the overall picture changes, our approach for how to solve problems will follow, too. 
- Let the solution guide the process and steps required to achieve it, it is about finding the way to the solution.

### Talent

- Children do not overcomplicate things the way that adults do.
- It is important that we influence the development of thought patterns, thought processes and recognition. This helps develop talent.
- Due to the difference in the variety of situations that can arise during penetration testing, it can be difficult to identify talent. This is because you will rarely be in the same situation twice (like Judo!) and it will take a considerable amount of time to develop and understanding of a bigger picture that can help drive investigative skills.
- Beyond biological predisposition, development of talent is more important, and can be achieved through carefully designed feedback loops that enable us to practice specific skills and understand their contexts.



