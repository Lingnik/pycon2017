# How documentation works, and how to make it work for your project
* "What nobody tells you about documentation"
* presenter: Daniele Procida
* divio.com - cloud hosting for python/django
* Django core dev
* Board member, Django Software Foundation
* @EvilDMP
* https://www.divio.com/en/blog/documentation/

## Better documentation will make your project more successful
* When you share your software with the world, you want the world to use it
* Many projects fall foul of this -- if your docs aren't up to the task, people won't use it
* "The project is just too much trouble." --> They'll go find something else to use.
* Without good docs, your "henchmen" won't want to use the software either.

## Most People Fail
* How can you make your documentation better?
    * Not by working harder at it
    * Not by making any more of it
    * By doing it the right way (an easier way to do it)
* Several simple, basic principles that aren't often spelled out (and have the status of secrets)
    * If you can use them, they'll make your docs better and your team more successful

## There are four things called documentation
* Four different purposes or functions
    1. Tutorials
    2. How-To Guides
    3. Discussions
    4. Reference
* Each requires a distinct mode of writing.
* People working with software need each of these four types at different times and circumstances.
* They are all necessary in your documentation.
* They must be kept entirely separate.
    * This makes it obvious to the author and the reader what goes where.
* Each quadrant has only one clear job.

### Tutorials
* Lessons that take the reader by the hand through a series of steps to complete a project (or meaningful exercise)
* Answer to a question that user might not be able to formulate.
* To show the reader "you can achieve something with this".
* Gives the reader the best possible start with practical knowledge.
* Oriented towards the users' learning:
    * Learning by doing
    * Getting started
    * Inspiring confidence- confidence in SW, docs, the teacher, and their own abilities
    * Repeatability- it must work every time for everybody in every circumstance
    * Immediate sense of achievement- must be comprehensible and meaningful, no matter how small it is
    * Concreteness, not abstraction- built around specific actions and outcomes, because learning comes from particular & concrete --> general & abstract
    * Minimum necessary explanation- you do not need to explain things! don't explain anything that the learner does not need to learn right now
    * No distractions-  don't distract the reader by options, choices, or alternatives

### How-To Guides
* Guides that take the reader through the steps required to solve a common problem.
* Answer to a user's questions.
* Problem-oriented
    * Specific, real-world problems.
    * Recipes / directions on how to achieve a specific end.
* What Matters
    * a series of steps- things that need to be done, in a particular order, to achieve a result
    * a focus on the goal- specific, practical goal; everything else is a distraction
    * addressing a specific question
    * no unnecessary explanation- no place for explanation. explanations get in the way of action
    * some flexibility- must have enough flexibility that a user can see how the guide can apply to a slightly different set of need
    * practical usability- more important than completeness. make some assumptions that your user already knows some things. don't mention everything that's related to this topic.
    * good naming- a topic like "how to create a class-based view" is a good title, but "creating class=based views" is worse, and "class-based views" is worst.

### Reference Guides
* Technical descriptions of the machinery and how to operate it.
* Things like key classes, functions, APIs.
* Just facts.
* Information oriented- examples as illustrations, but its job is not to explain basic concepts or explain how to achieve common tasks. be austere and get straight to the point.
* What Matters
    * structure- has the same structure as the "codebase" so you can navigate the product and the docs at the same time
    * consistency- as consistent tone, structure, format as possible across all the reference material as an encyclopedia would be
    * description- only has one job. describe the machinery as clearly and completely as possible. no explanation, discussion, opinion, because it's a distraction and will make it harder to maintain.
    * accuracy- must be accurate and up-to-date or the user will have information that is incorrect.
* This is often what devs think of re: docs. They already understand the docs, and all they can imagine users needing is some technical details about it, so that's all the write. Since they haven't made space for explanation, discussion, how-to guides, it makes the reference material less effective.

### Discussions
* Explanations that clarify and illuminate a particular topic.
* Oriented towards understanding.
* Chance for the docs to relax, step back from the software, take a wider view of it.
* What Matters
    * giving context- "web forms and how they are handled in django"
    * explaining why- e.g. design decisions, historical reasons, accidents
    * multiple examples, alternative approaches- even different/contrary opinions on a topic
    * making connections- help the user join the dots between different things that make sense in a subject
    * no instruction or technical description- not the place for instruction or telling people how to do things
* Not as easy to write; tutorials and reference docs are straightforward.
* Have to determine when/where to begin and end a discussion.

## The Four Purposes and Goals
* Tutorials- Learning-Oriented
* How-to Guides- Problem-Oriented
* Reference- Information-Oriented
* Discussions- Understanding-Oriented

### Quadrant Overlap
* Tutorials & How-To Guides- practical steps
* How-to guides & Reference- most useful when we're coding
* Discussions & reference- theoretical knowledge
* Discussions & tutorials- most useful when we're studying

## Why does this matter?
* It is the overlap of these goals that causes people to conflate purposes.
* Avoiding conflation will result in clearer documentation that is easier to maintain.
* If you do this, for each phase of the user's use if your software, they'll find the right documentation
* It will turn your learners into users who will use your software effectively.
* That is what the programmer wants most of all.

## Questions

### Where does StackOverflow fit? (how do you know what's missing?)
* Gives you indication of what might be missing.
* You get a sense of whether tutorials, how-to guides are a problem.

### What about documentation being replaced by docs?
* I completely disagree with the bot model of documentation.
* e.g. tutorials require a teacher leading a learner through a program that needs an advisor.

### As a writer of documentation & code, reference section is obvious to change when you make a change... how do you know you broke a tutorial?
* Tutorials are very difficult because a user has to go from the start to the end, and it HAS to work.
* If it doesn't work, the user flounders.
* Every week, someone has to go through the tutorial from beginning to end.
* It's just hard work.

### Adoption- how do you sell other engineers to conform to standards and documentation in general when the argument is "that's too hard"?
* This schema makes things easier.
* Shift documentation around first and put them in the right place.
* Include a preface that says "our documentation is written in this way" so readers and writers have something to follow.
* Django's documentation follows this--first page spells this structure out clearly.
    * https://docs.djangoproject.com/

### What metrics do you use to gauge the success of particular documentation types?
* Questions of a particular type have dramatically declined in our IRC channel.
* The docs were there! But in the wrong place.
