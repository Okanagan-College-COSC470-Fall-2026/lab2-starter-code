# Lab 2 - The Bowling Game Kata with a Branching Workflow

In this lab, we will look at a simple software engineering development practice, using TDD (Test Driven Design, which we will more formally introduce during out discussion of testing in a few weeks).  With TDD, the goal is to:

1. Develop the test for one small part of code (small steps)
2. Run test (the test should fail as you haven't written the actual code yet...)
3. Write code to fix and have test pass
5. Refactor (remove duplication and streamline testing)
4. Loop back and move onto next test/feature.

Concisely, with Test Driven Design you write your test first, then write the code.  In our workflow, you will need create a branch for each feature (each test + code to pass the test), review it, issues a PR and merge it back into the master (see image below). 
In the branch history, tests must be written first (commit your tests), reviewed and then the code written.   Once the tests pass, you can issue the PR, have your code reviewed (by one of your team members) and merged back into master.   Then you can move on to the next feature.   If you find an issue, create an issue in GitHub so it can be tracked and resolved.  A detailed discussion on TDD can be see [here](https://en.wikipedia.org/wiki/Test-driven_development).

![TDD (src: wiki)](https://upload.wikimedia.org/wikipedia/commons/thumb/0/0b/TDD_Global_Lifecycle.png/1024px-TDD_Global_Lifecycle.png)
(attr: Xarawn, CC BY-SA 4.0 <https://creativecommons.org/licenses/by-sa/4.0>, via Wikimedia Commons)

Of course, you might want to attempt to divide up the features that need to be implemented and tested.  You are welcome to do this, but the workflow **MUST** be the same where each feature needs to be on it's own branch and reviewed by one of your team members before being merged.   If you have multiple branches in action at the same time, make sure to pull frequently for any changes to avoid merge conflicts!!

The goal of this lab is to practice incremental software development by focusing on the immediate needs of the problem and working in small steps. With this approach, you reach and achieve the goals without (hopefully)
over complicating the problem.  One common issue that is encountered in software engineering is the is the development of overly complex solutions (more than is needed to solve the problem).  With this lab, part of the objectives is to practice identifying the key things that need to be tested and how to test the boundaries.  

For this to work, tests must first be written (and when you run them, they will fail because you have no code yet). Write your code so the tests pass. Refactor and improve.  

Refactoring or adding functionality should be done in a series of small steps and the testing will help to determine what next needs to be done or that the code is good.   Start by writing the the most straight forward code (i.e. can you instantiate the object), test, fix and refactor.  The goal here is to focus on the successful testing of the code and the development process (and less about how you solve the problem).  

## Important things:

1. How are you going to manage your software development process and assign tasks?   We haven't talked about different processes in detail, but have introduced some ideas.  As a team, decide and plan (we've talked about some options).   You will need to include tracking of the tasks (who is working on what).  This could be as simple as a text document that lists things out but needs to be included in root of your project (or a link/snapshot of the tool you used).  You can use the issue tracker in GitHub for this and a template has been provided if your team wishes to use it. 
2. How are you going to test your code?   You will need to explore how to write simple unit tests.  This project is written in, using the class and interface provided.  The starter code contains a JUnit test class.  Please do not change the names or placement of code in project. 
3. The project MUST be written in Java using the starter code and you will need to ensure that you implement the methods listed in the interface.  I set up this project using VSCode (the VSCode config is in the repo, but you are welcome to also use Eclipse).   

**Some helpful links**

The following items might help you when setting up VSCode.
- [https://code.visualstudio.com/docs/java/java-tutorial](https://code.visualstudio.com/docs/java/java-tutorial)
- [https://code.visualstudio.com/docs/java/java-testing](https://code.visualstudio.com/docs/java/java-testing)

If you are having issues running Java (you can test your installation by compiling and running Lab2.java) after getting Java setup in VSCode, this might be [helpful](https://github.com/microsoft/vscode-java-dependency/issues/179#issuecomment-1328975852)

## Remember! ##
**No Branches; no marks.**  It's not about getting the code done, but how you get the code done (mastering a flow).  Plan with your team, and reflect on it. 

## The Details

Phew!  After that long preamble, let's get onto the problem....

The problem that we will work on in this lab is a coding Kata.

> "Kata is a term used by some programmers in the Software Craftsmanship movement. Computer programmers who call themselves "Software Craftsmen" will write 'Kata' - small snippets of code that they write in one sitting, sometimes repeatedly, often daily, in order to build muscle memory and practice their craft." (src: https://en.wikipedia.org/wiki/Kata)

We will be using this exercise to work on building and developing our software engineering practices.   

This Kata is based on the Bowling Game Kata by Robert Martin (aka "Uncle Bob", known for the Agile Manifesto and other contributions to Agile development practices)(see: [The Bowling Game Kata](http://butunclebob.com/ArticleS.UncleBob.TheBowlingGameKata)).  In this exercise, the goal is to **iteratively** develop and solve the problem through the actions of developing solid tests, coding and refactoring, with the goal of building a program to score a bowling game.  

In order to better understand the problem, the following are the requirements for a bowling game (for 10 pin bowling):

- A game is made of 10 frames 
- For each frame, 10 pins are set and the player can roll 1 or 2 balls.  At the end of the turn, the pins are reset (new frame) (the count of the number of pins left that can be knocked down of a given frame)
- A player rolls the ball a maximum of twice during each frame
- The score is equal to the number of pins that are knocked down in a given frame (the total score is the points for all frames) but
    - If a player gets all the pins in the first roll for a frame (a **strike**), the frame ends.    
        - A **strike roll** has a score bonus: the sum of knocked pins of the next two rolls (you need to include the score for the next two rolls)
    - If a player knocks less than 10 pins down on the first roll, they get a second roll.
    - After their second roll (if they didn't get a strike on the first roll):
        - If they knocked all the pins down (what is left from the first roll), then this is a **spare*.  
            - A **spare** has a score bonus which is a number of knocked pins of the next roll.
        - If the player didn't knock all the pins down on their second roll, the score is just the number of pins that were knocked down (1 point per pin) for that frame.
    - If a **spare** is thrown in the final (10th) frame the player is awarded one more roll. This roll awards the number of pins cleared onto their score (0 to 10 bonus points).
    - If a **strike** is thrown in the final (10th) frame the player is awarded two more rolls. This roll awards the number of pins cleared onto their score (0 to 10 bonus points) for each throw

For each ball thrown and frame, you will need to keep track of the score.  You will also need to think about the number of pins left after a throw (a call to the rolls method).  This is the only method that needs to be called for the game to proceed, but you will need to keep track of what frame is being played (so you can possibly score for a potential spare or strike) within your code (there isn't a method for `frame`; based on how the rolls proceed, your code just needs to know what frame is being played).  For example, if you roll a 4 on the the first throw of a frame, then there are only 6 pins left (if you try to knock down more).  

**Here are some scenarios to consider with this game (hints!):**

1. If a player throws all **gutterballs** (no pins are knocked down is any of the frames), the score is 0.
2. The `roll` method is used to indicate the number of pins that have been knocked down for a given throw.  Think about boundary conditions (you can't knock down negative pins, or more pins than are available)
3. A perfect game has a score of **300**.  This is where a player throws 10 strikes (10 frames) and then gets two extra throws (as the score value for any frame which records a strike is 10 plus the value of the next two throws).     
4. Remember how many frames are in a game and the maximum number of possible throws available. 

**Suggestions for approaching your testing**

1. Start by testing to make sure that you can call the game constructor (ie test the game instantiation of `Game` first).
2. The move onto testing the easiest thing which is a game that is all gutter balls (where you knock over zero pins).  As you work though the problems you want to see your tests fail (because there is no code yet) and the work to make it pass.  Remember to check the score.
3. Consider a scenario where you bowl only 1 pin down for the entire game.
4. Consider a scenario where you bowl a 1 for each ball (20 throws of 1 for the 10 frames)
5. Consider a spare and it's possible scoring
6. Consider a strike and it's possible scoring
7. Consider the other scores possible and test. 

In each of these cases, consider the scoring and what's being tested (Hint: after you test the construction of the Game object, you might want to look at the @setup in JUNIT so you don't keep re-instantiating the Game object as one possible optimization).  Refactor frequently after you've committed, reviewed and merged each test.  

Also, if you have duplicated testing of specific operations as you develop thing, you will need to refactor and remove redundant test(s).  Think about how you create the game object (think about using @setup in the unit testing).  

You can create private methods/private instance data to help solve the problem but think about not over engineering this scenario.

**Hint:** The final solution for this problem is not overly complex (under 40 lines of code for the Game class).  Don't overthink and over-engineer the problem.  If you approach this problem in the test first/test basic conditions first, your team should be able to produce the final results.  

## Folder Structure and Code

You must use this project to complete the lab.   It includes some skeleton code as well as an interface for methods that must be implemented.  You are welcome to add other private methods and instance data to the Game class, but keep in mind that only `roll` and `score` will be tested (meaning that regardless of the order they are called in, they will be used to score and check an entire game of 10 frames). 
 
You will need to compete the `Game.java` and the `TestGame.java` classes.  You can test to make sure that your project is configured correctly by compiling and running the `Lab2.java` class (it's just a hello world for testing) as well as running the tests in `TestGame.java` that have a couple of quick tests (one passes and one fails) to ensure that your testing framework is working. 

The skeleton was build with Java 17 using JUnit4

The workspace contains two folders by default, where:

- `src`: the folder to maintain sources (add all your code here)
- `lib`: the folder to maintain dependencies (includes jar files for JUnit)

Meanwhile, the compiled output files will be generated in the `bin` folder by default.  You will want to add a `.gitignore` so that only your java files and the text document listing your planning and assignment of tasks gets committed to the repo (you will need to do some research on this).  

## Submissions

Make sure your team is pushing changes back to the repo on GitHub (which you will need to do anyway with your branching workflow as well as when dealing with PR's).  When you are done, in addition to having completed the tasks to complete the Bowling Game using TDD, make sure to answer the reflection question in a text or markdown file.  **Ensure that everything is merged to master after your tests pass and then push the results upstream to GitHub**.  You will need to submit the link of your repo to Moodle.  

## Scoring

- [+1] Task assignment/management 
- [+1] Updating `.gitignore` to ignore the indicated files
- [+6] Branching, testing, PR, merging (each person should work on equal parts of coding which includes the tests as well as the scoring).  Make sure to work on this in an organized and orderly fashion.  Make sure to note who reviews and clears PR.  The branches need to start with the easiest tests/code first (**hint: there are at minimum 6 test scenarios to consider** so there will need to be at least 6 branches in play for the duration of the project)
- [+6] Testing/coverage - Do your tests run/pass and test the key items in the challenge.  Consider the different cases presented.  Don't over test/duplicate testing. 
- [+3] Refactoring/optimization of testing; 
- [+3] Pass the secret tests! (We have a set of tests that we will run on the code) to ensure it is working that will only use the `roll` and `score` methods.
- [+1] Reflection: As a team, comment on what went well and what you would do differently if you were faced with this type of problem again.  Include your answer in a text or markdown doc in the repo.

**Total: 21 points (Assigned as team mark)**

## Final Thoughts

- Please ensure to add names of team members into the header on each file. 
- Do not delete branches (they will need to be in place for marking)
- Make sure that you commit your tests before committing code (this must be clearly visible in the commit history to earn the marks)
- Everyone needs to contribute
- Make sure to commit and push often
- Don't forget to pull!
- **Remember that this is a team assignment (3 people groups);** the TDD model presented must be used.  If you don't use the process, you will forfeit the marks for the lab regardless of what is produced (ie.  If one person writes all the code, commits it, creates PR that is just reviewed by the other member, the mark awarded will be 0).   

**No Branches; no marks.**  It's not about getting the code done, but how you get the code done (mastering a flow).  

**Good Luck and Happy Coding!**

