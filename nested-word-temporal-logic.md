**Date and Time**: 12/01/2017, Tuesday at 9:50 - 11:20  
**Presenter**: Hyeyoung Shin

## Nested Word Temporal Logic


###The papers

This presentation is based on *A temporal logic of nested calls and returns* by *Rajeev Alur, Kousha Etessami, and P. Madhusudan*. and two other papers by *Rajeev Alur*, which will be mentioned in the reference section.


###How does my presentation fit into this course?

In the course of this semester, we have studied various implementation techniques for formal static analysis such as abstract interpretation, data-flow analysis, type system, etc. Today, I would like to talk about another useful tool in static analysis, model checking.

Model checking is an automatic verification technique for finite state concurrent systems developed by Clarke and Emerson in early 1980's.

Model checking has two components, model of computation and specification language. A model checking problem then is

> Given a model of a system, exhaustively and automatically check whether this model meets a given specification formulated in logic.
>-wiki

In traditional model checking, the model is a finite state machine and specifications are written in propositional temporal logic. However, model checking is also available when the model is a recursive state machine, which is more appropriate for the purpose of the special specification language I am going to introduce to you today.  

Advantages of model checking are no proofs, fast, counterexamples (*Clarke*). The main disadvantage is combinatorial blow up if the state space known as state explosion problem.

###What is Temporal Logic?

Temporal logic is a special kind of modal logic. Modal logic studies different "modes" of truth and temporal logic provides ways to describe and reason about how the truth values of assertions change over time.

Applications of temporal logic are many. One in computer science is that it is a tool for specifying and verifying correctness of computer programs [*Pnueli* 83]. Especially, it has been proved its usefulness in nonterminating or continuously operating concurrent programs such as operating systems and network communication protocols. In this sense, it differentiates itself from Hoare Logic, which is another well-known specification and verification tool in computer science.

Temporal logic can be categorized along a number of axes: propositional vs first-order, global vs compositional, branching vs linear, point vs interval, and past vs future. The most popular and widely used temporal logic in computer science is the linear temporal logic. Also, it is closely related to the main topic of my presentation today. Thus, it is worthwhile to overview its syntax and semantics.

###What is the syntax and semantics of Linear Temporal Logic?

The underlying structure of time is discrete, infinite into the future, and has an initial moment. We can formalize this by $M = (S, x, L)$ where $S$ is the set of states, $x: \mathbb{N} \rightarrow S$ is infinite sequences of states or paths, and $L : S \rightarrow 2^{AP}$ is the labeling of true $AP$ at each state.  

The formulae of LTL are built from $AP$, logical connectives, and temporal operators
1. $\diamond$ $ **F** $\phi$   "**eventually** $\phi$"
2. $\square$ **G** $\phi$      "**always** $\phi$"
3. $\bigcirc$ **X** $\phi$     "**nexttime** $\phi$""
4. $\phi$ **U** $\psi$         "$\phi$ **until** $\psi$""

In the formal syntax **X** and **U** are the only primitive operators because the others can be written in abbreviation using **X** and **U**.

==========================================  
*Exercise 1:* Write **G** $\phi$ and **F** $\phi$ using **X** and **U**.  
==========================================

The binding precedence is all the temporal operators, $\neg$, $\wedge$, $\vee$, $\Rightarrow$.

==========================================  
*Exercise 2:* What is the difference between **GF** $\phi$ and **FG** $\phi$?  
==========================================  

We define semantics of a formula $\phi$ of LTL with respect to a linear time structure $M$. Also, note that $x_i$ denotes the $i$th state of $x$ and $x^i$ denotes the suffix of $x$ starting at the $i$th state of $x$.

$M, x \vDash \phi$ means in structure $M$, formula $\phi$ is true of x$.

$\vDash$ is inductively defined on the structure of the formulae.

$\cdot$ $x \vDash \phi$ iff $\phi \in L(s_0).$  (or $x \vDash \phi$ iff $x_0 \vDash \phi$.)

$\cdot$ $x \vDash \phi \wedge \psi$ iff $x \vDash \phi$ and $x \vDash \psi$.

$\cdot$ $x \vDash \neg \phi$ iff $x \nvDash \phi$.

$\cdot$ $x \vDash \phi$ **U** $\psi$ iff $\exists j (x^j \vDash \psi$ and $\forall k < j (x^k \vDash \phi)$.

$\cdot$ $x \vDash$ **X** $\phi$ iff $x^1 \vDash \phi$.

$\cdot$ $x \vDash$ **F** $\phi$ iff $\exists j (x^j \vDash \phi$).

$\cdot$ $x \vDash$ **G** $\phi$ iff $\forall j (x^j \vDash \phi$).

$\cdot$ $x \vDash$ **GF** $\phi$ iff $\forall k (\exists j \geq k) (x^j \vDash \phi$). ($\phi$ is true infinitely often.)

$\cdot$ $x \vDash$ **FG** $\phi$ iff $\exists k (\exists j > k) (x^j \vDash \phi$). ($\phi$ is almost always true. (finite number of times?))


###Can we extend LTL so that we can express truth of the past?

Yes, with past tense operator -.

1. **F-** $\phi$  "**sometime in the past** $\phi$ holds"
2. **G-** $\phi$  "**always in the past** $\phi$ holds"
3. **X-** $\phi$  "**lasttime** $\phi$ holds" (at $s_0$ false)
4. $\phi$ **U-$$** "**sometime in the past** $\psi$ holds and $\phi$ holds **until** then"


### Why CARET and how is it different from LTL?

The special specification language of interest is called Nested Word Temporal Logic. While LTL is good for capturing regular sequencing requirements such as "between successive write operations to a variable, a read operation should occur," it cannot express requirements such as "if the pre-condition $p$ holds when a module is invoked, the post condition $q$ should hold when the module returns". This requires matching of calls and returns, and is a non-regular property if calls are nested. (recall the first non-regular language $\{a^nb^n \mid n \in \mathbb{N}\}$)

*Rajeev Alur, Kousha Etessami*, and *P. Madhusudan* introduce CARET, a temporal logic that can express matching calls and returns. Calls here represent invocations of a program module and returns are the exit from the called module.

Besides global temporal modalities, CARET has abstract and caller counterparts for all the temporal modalities. Moreover, there are three kinds of paths each obtained by repetitive applications of global, abstract, and caller **succ^b** $b \in \{g, a, -\}$ operator.

1. The global paths is the usual path in the state space.

2. The abstract paths removes computation corresponding to calls to other blocks capturing the *local* computation within a module.

2. The caller path can be interpreted as the contents of the call-stack at a position.

CARET mixes global, abstract, and caller modalities allowing integrated and more expressive specification properties such as "variable $x$ remains unchanged after the current call returns."

The computation model of CARET is a combination of structured computation, recursive state machine, and Kripke structure, which will be defined soon.

The authors of the paper claim that CARET is the first specification language that allows specification of partial and total correctness with respect to pre and post conditions, e.g. total correctness w.r.t. the pre and post condition is the formula

$\square (call_A \wedge p \rightarrow \bigcirc^a q)$

 and has a decidable model checking problem.

Model checking problem of CARET is to check whether all computations of an RSM S satisfy a CARET specification $\phi$. Note that both $S$ and $\phi$ are context-free, but in their setting, the model checkin problem becomes decidable since the model and the specification are synchronized on the call and ret symbols.  


### The model of computation of CARET?

1. Structured computations

   Execution of a program is typically modeled as a word over an alphabet. We can then represent an infinite sequence of states of a structured computation with an infinite word $\alpha$ over alphabet $\hat{\Gamma}$ where $\Gamma = 2^{AP}$ and $\hat{\Gamma} = \Gamma \times \{call, ret, int\}$.

   For a word $\alpha$ over $\hat{\Gamma}$, there is a natural notion of a matching between calls and returns: if $\alpha_i = (\sigma, call)$ and $\alpha_j = (\sigma', ret)$, then $j$ is the matching return for $i$ if $j$ is the return corresponding to the call at $i$. More formally, we can define a function $R_{\alpha}$ that maps any $i \in \mathbb{N}$ to the first unmatched return after $i$. Otherwise, $R_{\alpha} = \bot$

   * $succ^g_{\alpha}(i) = i+1$
   * $succ^a_{\alpha}(i) = j > i$ the matching return otherwise $\bot$
   * $succ^{-}_{\alpha}(i) = j < i$ the innermost call otherwise $\bot$

2. Recursive state machines

   Recursive state machines model the interprocedural control flow in recursive programs. An RSM $S = (M, \{S_m\}_{m\in M}, Start)$.

   For each $m \in M$ $S_m$ is a module where $S_m = (N_m, B_m, Y_m, En_m, Ex_m, \delta_m, \eta_m)$.

  * $N_m$ the finite nonempty set of nodes
  * $B_m$ the finite set of boxes
  * $En_m \subseteq N_m$ the nonempty set of entry nodes
  * $Ex_m \subseteq N_m$ the nonempty set of exist nodes
  * $\delta_m : N_m \cup Retns_m \rightarrow 2^{N_m \cup Calls_m}$ is a transition function where  
         $Calls_m = \{(b, e) \mid b \in B_m, e \in En_{Y_m(b)}\}$ the set of calls of m  
         $Retns_m = \{(b, x) \mid b \in B_m, e \in Ex_{Y_m(b)}\}$ the set of returns of m  
  * $\eta_m : V_m \rightarrow 2^{AP}$ where   
        $V_m = N_m \cup Calls_m \cup Retns_m$ is a labeling function that associates a set of propositions to each vertex

     $V = \bigcup_{m \in M} V_m$  
     $B = \bigcup_{m \in M} B_m$  
     $\eta : V \rightarrow 2^{AP}$  
     $Y : B \rightarrow M$




3. Global Kripke structure $K_s = (Q, Init, \kappa, \delta)$.

  * $Q = \{(\gamma, u) \mid \gamma \in B*, u \in V\}$ such that  
  either  
       $\gamma = \epsilon$ and $u \in V$
       $\gamma = b_1.b_2...b_k (k \geq 1)$ and $\forall i \in [a, k-1] (b_{i+1} \in B_{Y(b_i)}$ and $u \in V_{Y(b_i)}$  
 * $Init = \{(\epsilon, u) \in Q \mid u \in start\}$
 * $\kappa((\gamma,u)) = (\eta(u), z)$ where  
         $z = int$ if $u$ is a node  
         $z = call$ if $u$ is a call  
         $z = ret$ if $u$ is a return  
* $\delta : Q \rightarrow 2^Q$ is defined by $(\gamma, u) \mapsto (\gamma', u')$ if  
      1. internal move: $u \in (N_m \cup Retns_m) \ Ex_m \rightarrow u' \in \delta_m(u)$ and $\gamma' = \gamma$  
      2. call a module: $u = (b,e) \in Calls \rightarrow u' = e$ and $\gamma' = \gamma.b$  
      3. return from a call: $u \in Ex_m \rightarrow \gamma = \gamma'.b$ and $u' = (b,u)$


For a word $\alpha = \alpha_0 \alpha_1 \alpha_2 ... \in \hat{\Gamma}^{\omega}$ a run of $K_s$ on $\alpha$ is a sequence of states $\pi = s_0, s_1, ... $ where $\kappa(s_i) = \alpha_i \forall i \in \mathbb{N}$ such that $s_o \in Init$ and $\forall i \in \mathbb{N} (s_{i+1} \in \delta(s_i))$.

$L(S) = \{ \alpha \in \hat{\Gamma}^{\omega} \mid \exists \text{ run of } K_s \text{ on } \alpha \}$



###Finally CARET

Let $P = AP \cup \{call, ret, int\}$. The models of the logic are the words in $\hat{\Gamma}^{\omega}$. Then CARET over $AP$ is the set of formulas defined by  

$\phi := p \mid \phi \vee \phi \mid \neg \phi \mid \bigcirc^g \phi \mid \phi U^g \phi \mid \bigcirc^a \phi \mid \phi U^a \phi \mid \bigcirc^- \phi \mid \phi U^- \phi$

For a word $\alpha \in \hat{\Gamma}^{\omega}$, the semantics is inductively defined by $(\alpha, n) \vDash \phi$ where $n \in \mathbb{N}$. A word $\alpha$ satisfies $\phi$ iff $(\alpha, 0)$ satisfies $\phi$.

   * $(\alpha, n) \vDash p$ iff $\alpha_ n = (X, d)$ and $p \in X$ or $p = d$ (where $p \in P$)
   * $(\alpha, n) \vDash \phi_1 \vee \phi_2$ iff (\alpha, n) \vDash \phi$ or $(\alpha, n) \vDash \phi_2$
   * $(\alpha, n) \vDash \neg \phi$ iff $(\alpha, n) \nvDash \phi$  
   * $(\alpha, n) \vDash \bigcirc^g \phi$ iff $(\alpha, succ^g_{\alpha}(n)) \vDash \phi$, i.e., iff $(\alpha, n+1) \vDash \phi$
   * $(\alpha, n) \vDash \bigcirc^a \phi$ iff $succ^a_{\alpha(n)} \neq \bot$ and $(\alpha, succ^a_{\alpha}(n)) \vDash \phi$  
   * $(\alpha, n) \vDash \bigcirc^- \phi$ iff $succ^{-}_{\alpha} \neq \bot$ and $(\alpha, succ^{-}_{\alpha}(n)) \vDash \phi$
   * $(\alpha, n) \vDash \phi_1 U^b \phi_2$ for any $b \in \{g, a, -\}$ iff there is a sequence of positions $i_0, i_1, ..., i_k$, where $i_0 = n$, $(\alpha, i_k) \vDash \phi_2$ and for every $0 \leq j \leq k-1$, $i_{j+1} = succ^b_{\alpha}(i_j)$ and $(\alpha, i_j) \vDash \phi_1$


Then the model-checking problem statement is   
>Given an RSM S and a formula $\phi$ of CARET, does $S \vDash \phi$?


================================================  
Exercise 3. Consider the RSM S. Assume $start = En_1$.  
 $S \vDash \square^g (d \rightarrow \diamond^g z)?

 How about  $S \vDash \square^g (d \rightarrow \diamond^a z)$?

 How about $S \vDash \square^g (y \rightarrow \bigcirc^- t)$?  

================================================




###The expressiveness of CARET

  1. Pre and Post conditions
     * $\phi_{total} : \square [(call \wedge p \wedge p_A) \rightarrow \bigcirc^a q]$  
     * $\phi_{partial} : \square [(call \wedge p \wedge p_A) \rightarrow \neq \bigcirc^a \neq q]$

  2. Boundedness
     * $\phi_{empty} : \square (call \rightarrow \bigcirc^a ret)$
     * $\phi_{rep-bounded} : \diamond\square (call \rightarrow \bigcirc^a ret)  

  3. Local Properties
     * $\phi_{local-response} : \square [(t_A \wedge p) \rightarrow \diamond^a q]$

  4. Stack Inspection Properties
     * $\phi_{stack} : \square (call \wedge p_A \rightarrow (\neg p_C) U^- p_B)$

Of course, the expressiveness of CARET is limited. For example, we won't be able to express the halting problem in CARET. In return, we have decidable model checking problems.

### HOWTO of model checking
