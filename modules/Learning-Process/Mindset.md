# I Want to Hack the Box!

## Learning Process

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

------

## Learning Process : Learning Dependencies

March 30th, 2024

### Way of Learning

- Problems are an emotional state. Without emotions, it is just a situation.
- An essential part of what makes you successful is knowing your goal, because these goals will largely help you define which actions you take along the way (think about how we talked about how to use [Occam's Razor](#Occam's Razor)). We search for a solution, letting our goal of solving this solution (and using the most probable cause) guide our decision making.
- If you do not know your goal, you can easily make decisions that are counterproductive, e.g. "chair in the middle of the room" example.

### What is my goal for this course?

#### TODO: 500 words specific on what I want to get out of this course?

------

### Learning Efficiently

- In order to combine knowledge, adaptation, and information in order to solve problems, we must try to understand both what we know and what we don't know.
- Failure is an unavoidable and essential process to learning.
- Pareto principle - with 20% of the effort, we can achieve 80% of the effect; with 80% effort, we can achieve the remaining 20%, which is 100% missing. (Note to self: dunno what this means, but it sounds like "60% of the time it works every time")
- **Progress is noticeable when the question that tortured us has lost its meaning.**

### Learning Pyramid

- Different kinds of learning have different "efficiencies" or uses for learning. From a memory standpoint, reading, listening, and seeing are weaker, and writing, saying, sing and hearing, and doing are stronger. Each of these modes of learning lets us process information differently, and should be used with their efficiencies in mind. E.g., reading over something that is completely new to you will be helpful, but there's only so much you can get from reading literature. It can help reinforce knowledge, but you should be practicing and trying even if you can't do much for the experience of doing it.

- you don't only need to practice the skill, you need to practice doing the skill efficiently, which in itself is different, and can be heavily influenced by goals and application of occam's razor
- creativity causes our brain to behave differently

### Questions

- At the moment, we define questions and see their purpose as gathering information and facts from which we can draw conclusions and make assumptions that will guide our decisions and thus our future course of action.
  - The most important and most difficult thing in any situation is  not the search for the right answer but the search for the right question
  - It is challenging to seek the right questions when we do not understand the concepts or do now have any knowledge in a particular area in the first place.

### Assessing Question Quality

- How to we structure questions with different levels of detail to get information.
- How do we ask relevant questions, how does describing a situation help us apply pareto principle and occam's razor towards finding a solution.
- Questions are based off of origin, process, result/goal.
- We can design questions with creativity to pursue different avenues of information
- We can consider what questions have in common, and connect those components

```
A right question is a precise question that allows us to establish the relationships between the components, to understand them, and to  take us one step further to the required answer.
```

I wonder if these questions should try to be atomic in nature. It seems like the authors are trying to define a system for framing questions in useful steps in the pursuit of information, which feels a lot like structuring code. 

### Frustration and Success

- Enumeration is key, it is important to have a clear, well defined goal. We can see how the lack of a specific element of our lives can effect our emotions.
- Vision -> Skills -> Incentives -> Resources -> Action Plan -> ORGANIZATIONAL SUCCESS
  - If you are lacking one of the nodes in this linked list, it can lead to a different outcome. Try writing down something for each of these values when trying to come up with a goal for yourself, and you can see what you need to do to fill in the gap.
- It is important to deal with frustration in a controlled and conscious way. Frustration is temporary.

### Keeping Track of Progress

- Progress does not always manifest, but it is critical to be consistent. Change happens more quickly when you are immersed in something, but more important, it is about exposure to more context that makes our practice more powerful. This is also like judo.
- Other people are often bad judges of progress, but that doesn't mean you can't watch for progress and use progress tracking to see it yourself and use it as a way of improving.

**Exercise:**

```
List No. 1

On the first list, you write down the current date and everything you know about your desired topic with all your skills with an estimated scale of 1-10. Try to make it as detailed as possible. The more detailed it is, the clearer the difference will be for you to see later. As soon as you think this list is ready, put it down or save it in a way that you will have access to it even after one year.

List No. 2

The second list is written continuously. This means that as soon as you have familiarized yourself with a topic and you have learned something new for yourself, you will add it to this list. Try to learn every day, even if it takes only 10 minutes. If you want to do it more scientifically to get even better results, document the calendar weeks.
```

This might seem like overkill, but being able to know exactly what you know and what you don't know is imperative for targeting areas of improvement. We are rarely trustworthy narrators in our own heads.
