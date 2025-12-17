---
title: Test-Driven Development By Example
---
# Test-Driven Development By Example by Kent Beck

## Preface

### Red/green/refactor—the TDD mantra

- Red
	- Write a little test that doesn't work, and perhaps doesn't even compile at first.
- Green
	- Make the test work quickly, committing whatever sins necessary in the process.
- Refactor
	- Eliminate all of the duplication created in merely getting the test to work.

### Red/green/refactor—the TDD mantra

Assuming for the moment that such a programming style is possible, it further might be possible to dramatically reduce the defect density of code and make the subject of work crystal clear to all involved. If so, then writing only that code which is demanded by failing tests also has social implications.

- If the defect density can be reduced enough, then quality assurance (QA) can shift from reactive work to proactive work.
- If the number of nasty surprises can be reduced enough, then project managers can estimate accurately enough to involve real customers in daily development.
- If the topics of technical conversations can be made clear enough, then software engineers can work in minute-by-minute collaboration instead of daily or weekly collaboration.
- Again, if the defect density can be reduced enough, then we can have shippable software with new functionality every day, leading to new business relationships with customers.

### Courage

Imagine programming as turning a crank to pull a bucket of water from a well. When the bucket is small, a free-spinning crank is fine. When the bucket is big and full of water, you're going to get tired before the bucket is all the way up. You need a ratchet mechanism to enable you to rest between bouts of cranking. The heavier the bucket, the closer the teeth need to be on the ratchet.

The tests in test-driven development are the teeth of the ratchet. Once we get one test working, we know it is working, now and forever. We are one step closer to having everything working than we were when the test was broken. Now we get the next one working, and the next, and the next. By analogy, the tougher the programming problem, the less ground that each test should cover.

## Introduction

You can't control whether you ever get the motive to multiply the value of your project by spinning technical magic. Method and opportunity, on the other hand, are entirely under your control.

Ward and his team created method and opportunity through a combination of superior talent, experience, and discipline. Does this mean that if you are not one of the ten best software engineers on the planet and don't have a wad of cash in the bank so you can tell your boss to take a hike, then you're going to take the time to do this right, that such moments are forever beyond your reach?

No. You absolutely can place your projects in a position for you to work magic, even if you are a software engineer with ordinary skills and you sometimes buckle under and take shortcuts when the pressure builds. Test-driven development is a set of techniques that any software engineer can follow, which encourages simple designs and test suites that inspire confidence. If you are a genius, you don't need these rules. If you are a dolt, the rules won't help. For the vast majority of us in between, following these two simple rules can lead us to work much more closely to our potential.

- Write a failing automated test before you write any code.
- Remove duplication.

## Preface

### Red/green/refactor—the TDD mantra

Assuming for the moment that such a programming style is possible, it further might be possible to dramatically reduce the defect density of code and make the subject of work crystal clear to all involved. If so, then writing only that code which is demanded by failing tests also has social implications.

### Courage

Imagine programming as turning a crank to pull a bucket of water from a well. When the bucket is small, a free-spinning crank is fine. When the bucket is big and full of water, you're going to get tired before the bucket is all the way up. You need a ratchet mechanism to enable you to rest between bouts of cranking. The heavier the bucket, the closer the teeth need to be on the ratchet.

The tests in test-driven development are the teeth of the ratchet. Once we get one test working, we know it is working, now and forever. We are one step closer to having everything working than we were when the test was broken. Now we get the next one working, and the next, and the next. By analogy, the tougher the programming problem, the less ground that each test should cover.

## Chapter 1 : Basics

- Add a little test.
- Run all tests and fail.
- Make a little change.
- Run the tests and succeed.
- Refactor to remove duplication.

## Chapter 2 : Degenerate Objects

The general TDD cycle goes as follows

1. Write a test
	- Think about how you would like the operation in your mind to appear in your code. You are writing a story. Invent the interface you wish you had. Include all of the elements in the story that you imagine will be necessary to calculate the right answers.
2. Make it run.
	- Quickly getting that bar to go to green dominates everything else. If a clean, simple solution is obvious, then type it in. If the clean, simple solution is obvious but it will take you a minute, then make a note of it and get back to the main problem, which is getting the bar green in seconds. This shift in aesthetics is hard for some experienced software engineers. They only know how to follow the rules of good engineering. Quick green excuses all sins.
3. Make it right.
	- Now that the system is behaving, put the sinful ways of the recent past behind you. Step back onto the straight and narrow path of software righteousness. Remove the duplication that you have introduced, and get to green quickly.

## Chapter 6. Equality for All, Redux

**You will often be implementing TDD in code that doesn't have adequate tests (at least for the next decade or so). When you don't have enough tests, you are bound to come across refactorings that aren't supported by tests. You could make a refactoring mistake and the tests would all still run. What do you do?**

- Write the tests you wish you had. If you don't, you will eventually break something while refactoring. Then you'll get bad feelings about refactoring and stop doing it so much. Then your design will deteriorate.

## Chapter 32. Mastering TDD

### How large should your steps be?

You could write the tests so they each encouraged the addition of a single line of logic and a handful of refactorings. You could write the tests so they each encouraged the addition of hundreds of lines of logic and hours of refactoring. Which should you do?

Part of the answer is that you should be able to do either. The tendency of Test-Driven Developers over time is clear, though—smaller steps.

At first when you refactor, you should be prepared to take lots of little tiny steps. Manual refactoring is prone to error, and the more errors you make and only catch later, the less likely you are to refactor. Once you've done a refactoring 20 times by hand in little tiny steps, experiment with leaving out some of the steps.

Automated refactoring accelerates refactoring enormously. What would have taken you 20 manual steps now becomes a single menu item. An order of magnitude change in quantity generally constitutes a change in quality, and this is true of automated refactoring. When you know you have the support of an excellent tool, you become much more aggressive in your refactorings, trying many more experiments to see how the code wants to be structured.

### What don't you have to test?

The simple answer, supplied by Phlip is, **"Write tests until fear is transformed into boredom."** This is a feedback loop, however, and it requires that you find the answer yourself. Because you came to this book for answers, not questions (in which case you're already reading the wrong section, but enough of the self-referential literary recursion stuff), try this list. You should test:

- Conditionals
- Loops
- Operations
- Polymorphism

### How do you know if you have good tests?

1. **Long setup code** : If you have to spend a hundred lines creating the objects for one simple assertion, then something is wrong. Your objects are too big and need to be split.
2. **Setup duplication** : If you can't easily find a common place for common setup code, then there are too many objects too tightly intertwined.
3. **Long running tests** :  TDD tests that run a long time won't be run often, and often haven't been run for a while, and probably don't work. Worse than this, they suggest that testing the bits and pieces of the application is hard. Difficulty testing bits and pieces is a design problem, and needs to be addressed with design.
4. **Fragile tests :** Tests that break unexpectedly suggest that one part of the application is surprisingly affecting another part. You need to design until the effect at a distance is eliminated, either by breaking the connection or by bringing the two parts together

### Can you test drive enormous systems?

Does TDD scale to extremely large systems? What new tests would you have to write? What new kinds of refactorings would you need?

The largest, totally test-driven system I've been involved with is at LifeWare ([www.lifeware.ch](http://www.lifeware.ch/)). After 4 years and 40 person/years, the system contains approximately 250,000 lines of functional code and 250,000 lines of test code in Smalltalk. There are 4,000 tests, executing in under 20 minutes. The full suite is run several times each day. The amount of functionality in the system seems to have no bearing on the effectiveness of TDD. By eliminating duplication, you tend to create more smaller objects, and those objects can be tested in isolation independent of the size of the application.

### How do you switch to TDD midstream?

You have a bunch of code that more or less works. You want to test drive your new code. What do you do next?

The biggest problem is that code that isn't written with tests in mind typically isn't very testable. The interfaces aren't designed, so it is easy for you to isolate a little piece of logic, run it, and check its results.

"Fix it," you say. Yes, well, but refactoring (without automated tools) is likely to result in errors, errors that you won't catch because you don't have the tests. Chickens and eggs. Catch-22. Mutually assured destruction. What do you do?

What you don't do is go write tests for the whole thing and refactor the whole thing. That would take months, months in which no new functionality would appear. **Spending money without making it is generally speaking not a sustainable process.**

So first we have to decide to limit the scope of our changes. If we see parts of the system that could be dramatically simplified but that don't demand change at the moment, then we will leave them alone. Shed a tear, perhaps, for the sins of the past, but leave them alone.

Second, we have to break the deadlock between tests and refactoring. We can get feedback other ways than with tests, like working very carefully and with a partner. We can get feedback at a gross level, like system-level tests that we know aren't adequate but give us some confidence. With this feedback, we can make the areas we have to change more accepting of change. Over time, the parts of the system that change all the time will come to look test driven.

### Who is TDD intended for?

Every programming practice encodes a value system, explicitly or implicitly. TDD is no different. If you're happy slamming some code together that more or less works and you're happy never looking at the result again, TDD is not for you. TDD rests on a charmingly naïve geekoid assumption that if you write better code, you'll be more successful. TDD helps you to pay attention to the right issues at the right time so you can make your designs cleaner, you can refine your designs as you learn.

I say "naïve," but that's perhaps overstating. What's naïve is assuming that clean code is all there is to success. Good engineering is maybe 20 percent of a project's success. Bad engineering will certainly sink projects, but modest engineering can enable project success as long as the other 80 percent lines up right. From this perspective, TDD is overkill. It lets you write code with far fewer defects and a much cleaner design than is common in the industry. However, those whose souls are healed by the balm of elegance can find in TDD a way to do well by doing good.

TDD is also good for geeks who form emotional attachments to code. One of the great frustrations of my young engineer's life was starting a project with great excitement, then watching the code base decay over time. A year later I wanted nothing more than to dump the now-smelly code and get on to the next project. TDD enables you to gain confidence in the code over time. As tests accumulate (and your testing improves), you gain confidence in the behavior of the system. As you refine the design, more and more changes become possible. My goal is to feel better about a project after a year than I did in the starry-eyed beginning, and TDD helps me achieve this.

### Why does TDD work?

Prepare to leave the galaxy. Let's assume for the moment that TDD helps teams productively build loosely coupled, highly cohesive systems with low defect rates and low cost maintenance profiles. (I'm claiming no such thing in general, but I trust you to imagine impossible things.) How could such a thing happen?

Part of the effect certainly comes from reducing defects. The sooner you find and fix a defect, the cheaper it is, often dramatically so (just ask the Mars Lander). There are plenty of secondary psychological and social effects from reduced defects. My own practice of programming became much less stressful when I started with TDD. No longer did I have to worry about everything at once. I could make this test run, and then all the rest. Relationships with my teammates became more positive. I stopped breaking builds, and people could rely on my software to work. Customers of my systems became more positive, too. A new release of the system just meant more functionality, not a host of new defects to identify among all of their old favorite bugs.

Reduced defects. Where do I get off claiming such a thing? Do I have scientific proof?

No. No studies have categorically demonstrated the difference between TDD and any of the many alternatives in quality, productivity, or fun. However, the anecdotal evidence is overwhelming, and the secondary effects are unmistakable. Programmers really do relax, teams really do develop trust, and customers really do learn to look forward to new releases. "By and large," I will say, although I haven't seen the opposite effect. Your mileage may vary, but you'll have to try it to find out.

Another advantage of TDD that may explain its effect is the way it shortens the feedback loop on design decisions. The feedback loop for implementation decisions is obviously short —seconds or minutes, followed by rerunning the tests tens or hundreds of times a day. The loop for design decisions goes between the design thought—perhaps the API should like this, or perhaps the metaphor should be that—and the first example, a test that embodies that thought. Rather than designing and then waiting weeks or months for someone else to feel the pain or glory, feedback comes in seconds or minutes as you try to translate your ideas into a plausible interface.

A weirder answer to, "Why does TDD work?" comes from the fevered imagination of complex systems. The inimitable Phlip says:

> Adopt programming practices that "attract" correct code as a limit function, not as an absolute value. If you write unit tests for every feature, and if you refactor to simplify code between each step, and if you add features one at a time and only after all the unit tests pass, you will create what mathematicians call an "attractor." This is a point in a state space that all flows converge on. Code is more likely to change for the better over time instead of for the worse; the attractor approaches correctness as a limit function.
This is the "correctness" that nearly all programmers get by with (except, of course, for medical or aerospace software). But it's better to explicitly understand the attractor concept than deny it or disregard its importance.
