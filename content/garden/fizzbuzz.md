+++
author = "VoidSpaceXYZ"
title = "The typical Mistake in a FizzBuzz Problem"
date = "2015-12-18"
description = ""
draft = false
tags = [
    "python",
    "interview"
]
categories = [
    "python",
]
series = []
+++



My first interview question to anybody, their responses and my learnings

This is one such interview question that no fresher has answered right in the first attempt. After about taking a few 10 interview candidates at my workplace, I am actually suprised. I was able to get it right the fist attempt.

So what is the question ?

> Write a program that prints the numbers from 1 to 100. But for multiples of three print “Fizz” instead of the number and for the multiples of five print “Buzz”. For numbers which are multiples of both three and five print “FizzBuzz”.

That is a simple question, but a lot of them get it wrong the first time. So what do people write as solutions ?

```plaintext
:::python for i in range(100): if i % 3 == 0: print "Fizz" elif i % 5 == 0: print "Buzz" elif (i % 3 == 0) and (i % 5 ==0): print "FizzBuzz" else: print i
```

Well this is the code, that all the people I have talked to have written in their first attempt. I used to actually be surprised initially but then I started expecting this answer.

So what is wrong here ? What are we to learn from a code like this ?

First, we fail to realize that we are writing a set of instructions,  a computer is going to execute. Well a computer is a brain less machine. Yes it does not have a brain like you and me. So any instruction given to a computer has to be step by step. It has to be logical.

Second, we dont think like a computer. We think like human beings. The above logic when thought like a human, will work perfectly. Why ? we apparently apply a certain amount of logic on top of a logic. Yep ! Knowingly or unknowingly we do that.

When I say the code is wrong to an interview candidate, I usually see this reaction in their face "Hey ! Hold on !! What is wrong with that code ?"

Simple. That is when I say "Execute your logic for the first 20 numbers." Yep, you could try too. If you still get the output right for the first 20 number on the above code, again read up the second point I have talked about. "Think like a computer, not like a human being" .

If you still get it right, dont worry , here is why it is wrong.

1.  Lets take 3. The above code will print "Fizz" . Why ? The first if condition satisfies.
2.  Lets take 5. The above code will print "Buzz". Why ? The second condition satisfies.
3.  Lets take 15. Why 15 ? It is the first number divisible from both 3 and 5. The above code will print "FizzBuzz". Why ? The third condition satisfies.

Right ! Hey hold on ...! Something is fishy here.  The third condition .. Isint that wrong ?? Wouldnt that print "Fizz" instead of "FizzBuzz" ? If this thought has come to you, yes you are right.

Lets analyze a little more. What does an "if" statement do ?

When a condition is satisfied, it executes a set of instructions. Right ? Now for a moment lets think like machines and look at the above code.

For the number 15, lets run though the first condition. 15 is divisible by 3. Yes the condition is true. So it would print "Fizz". Thats it. That is how a machine works.

That is the difference between "Natural Human Language" and "Computer Language".

> A computer can just execute instruction given by you and me. It does not have a brain by itself.

So without further delay, lets get to the solution.

```plaintext
:::python for i in range(100): if (i % 3 == 0) and (i % 5 ==0): print "FizzBuzz" elif i % 3 == 0: print "Fizz" elif i % 5 == 0: print "Buzz" else: print i
```

You see the re-arrangement in the logic ? Now run though the three numbers 3,5,15 and the code should work good.

(Psst ! Can you optimize the code a little more ?)

The point is very simple. When a problem has an ambiguity in terms of condition, always double think the conditions.

And as always, when you write code

> Always think like a computer.
