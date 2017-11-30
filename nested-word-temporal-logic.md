**Date and Time**: 12/01/2017, Tuesday at 9:50 - 11:20  
**Presenter**: Hyeyoung Shin

### Nested Word Temporal Logic


#The papers

This presentation is based on *A temporal logic of nested calls and returns* by *Rajeev Alur, Kousha Etessami, and P. Madhusudan*. and two other papers by *Rajeev Alur*, which will be mentioned in the reference section.


#How does my presentation fit into this course?

In the course of this semester, we have studied various implementation techniques for formal static analysis such as abstract interpretation, data-flow analysis, type system, etc. Today, I would like to talk about another useful tool in static analysis, model checking.

Model checking is an automatic verification technique for finite state concurrent systems developed by Clarke and Emerson in early 1980's.

Model checking has two components, model of computation and specification language. A model checking problem then is

> Given a model of a system, exhaustively and automatically check whether this model meets a given specification formulated in logic.
>-wiki

In traditional model checking, the model is a finite state machine and specifications are written in propositional temporal logic. However, model checking is also available when the model is a recursive state machine, which is more appropriate for the purpose of the special specification language I am going to introduce to you today.  

Advantages of model checking are no proofs, fast, counterexamples (*Clarke*). The main disadvantage is combinatorial blow up if the state space known as state explosion problem.

#What is Temporal Logic?

Temporal logic is a special kind of modal logic. Modal logic studies different "modes" of truth and temporal logic provides ways to describe and reason about how the truth values of assertions change over time.

Applications of temporal logic are many. One in computer science is that it is a tool for specifying and verifying correctness of computer programs [*Pnueli* 83]. Especially, it has been proved its usefulness in nonterminating or continuously operating concurrent programs such as operating systems and network communication protocols. In this sense, it differentiates itself from Hoare Logic, which is another well-known specification and verification tool in computer science.

Temporal logic can be categorized along a number of axes: propositional vs first-order, global vs compositional, branching vs linear, point vs interval, and past vs future. The most popular and widely used temporal logic in computer science is the linear temporal logic. Also, it is closely related to the main topic of my presentation today. Thus, it is worthwhile to overview its syntax and semantics.

#What is the syntax and semantics of Linear Temporal Logic?

The underlying structure of time is discrete, infinite into the future, and has an initial moment. We can formalize this by $M = (S, x, L)$ where $S$ is the set of states, $x: \mathbb{N}$








The special specification language of interest is called Nested Word Temporal Logic.
