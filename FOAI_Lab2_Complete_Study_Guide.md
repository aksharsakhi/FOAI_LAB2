# 23CSE401 Fundamentals of AI: Lab 2 Master Study Guide
## First-Order Logic (FOL) up to Inference in SWI-Prolog

> **Notice:** There are **NO CHEAT SHEETS** allowed in the exam. Everything in this guide is structured to help you memorize the syntax patterns, rules, and logic fast.

---

## Table of Contents
1. [Core Syntax Rules (The "No Cheat Sheet" Traps)](#1-core-syntax-rules-the-no-cheat-sheet-traps)
2. [SWI-Prolog Console & Shell Operations](#2-swi-prolog-console--shell-operations)
3. [The 5 Exam Programs (Full Code + Queries)](#3-the-5-exam-programs-full-code--queries)
   - [Program 1: Family Tree Logic (`family.pl`)](#program-1-family-tree-logic-familypl)
   - [Program 2: Cafe Database & Budget Arithmetic (`cafe.pl`)](#program-2-cafe-database--budget-arithmetic-cafepl)
   - [Program 3: Course Prerequisites & Recursion (`courses.pl`)](#program-3-course-prerequisites--recursion-coursespl)
   - [Program 4: Flight Network & Layover Duration (`airport_flights.pl`)](#program-4-flight-network--layover-duration-airport_flightspl)
   - [Program 5: Colonel West / Criminal Problem (`criminal.pl`)](#program-5-colonel-west--criminal-problem-criminalpl)
4. [Mastering Recursion (The Universal Template)](#4-mastering-recursion-the-universal-template)
5. [How Prolog Works: Viva & Theory Questions](#5-how-prolog-works-viva--theory-questions)
6. [Exam Quick-Check & Practice Drill](#6-exam-quick-check--practice-drill)

---

## 1. Core Syntax Rules (The "No Cheat Sheet" Traps)

Prolog's syntax is minimal, but strictly enforced. If you make any of the mistakes below, your code will fail to compile or give wrong answers.

| Concept | Syntax | Meaning / Example | Fatal Error to Avoid |
| :--- | :--- | :--- | :--- |
| **Constants / Atoms** | Starts with **lowercase** | `john`, `sarah`, `pizza`, `cs101` | Writing `John` or `Pizza` turns them into variables! |
| **Variables** | Starts with **UPPERCASE** or `_` | `X`, `Person`, `Child`, `Budget` | Writing `x` or `person` treats them as fixed constants, not variables. |
| **Rule Operator ("IF")** | `:-` | `Head :- Body.` (Head is true **if** Body is true) | Writing `=:` or `->`. |
| **Conjunction (AND)** | `,` (comma) | `parent(X, Y), male(X).` | Using `AND` or `&&`. |
| **Disjunction (OR)** | `;` or multiple clauses | `clause1 ; clause2.` | Usually best to write two separate rules with the same name. |
| **Statement Terminator** | `.` (period) | `male(john).` | **Every single fact, rule, and query MUST end with a period.** |
| **Arithmetic Evaluation** | `is` | `TotalCost is Price1 + Price2.` | **Never use `=` for arithmetic!** `=` is unification (pattern matching), not calculation. |
| **Less Than or Equal** | `=<` | `Price =< Budget.` | **Do NOT write `<=`.** Prolog only accepts `=<`. |
| **Greater Than or Equal**| `>=` | `Duration >= 5.0.` | Standard `>=`. |
| **Inequality / Not Equal** | `\=` or `\==` | `X \= Y` | Prevents a person from being their own sibling. |
| **Negation (NOT)** | `\+` | `\+ parent_of(Y, _)` | "Negation as Failure" (cannot be proved from database). |
| **Anonymous Variable** | `_` (underscore) | `item(Name, _, Price)` | Use when you don't care about a value in a fact. |

---

## 2. SWI-Prolog Console & Shell Operations

When running SWI-Prolog in the lab terminal or GUI:

### 1. Loading Files
Save your code in a file ending with `.pl` (e.g., `family.pl`). In the SWI-Prolog prompt:
```prolog
?- consult('family.pl').
% OR shorthand:
?- ['family.pl'].
```

### 2. Stepping Through Results
When Prolog prints an answer:
- Press **`;`** (semicolon) or **`Space`** to find the **next** alternative solution (backtracking).
- Press **`Enter`** to accept the current answer and return to the prompt.

### 3. Essential Built-in Commands
```prolog
?- listing.                  % Displays all loaded facts and rules in memory.
?- listing(parent).          % Displays only the facts and rules for 'parent'.
?- trace.                    % Enables step-by-step debugger for queries.
?- notrace.                  % Turns off trace mode.
?- write('\33\[2J').         % Clears the terminal screen.
?- halt.                     % Exits SWI-Prolog.
% If execution gets stuck in an infinite loop: press Ctrl+C, then type 'a' (abort).
```

---

## 3. The 5 Exam Programs (Full Code + Queries)

These are the exact 5 scenarios practiced in the lab syllabus.

---

### Program 1: Family Tree Logic (`family.pl`)

#### Problem Statement
> John and Sarah are married and have two children together: Mary and Tom. Mary later has a daughter named Ann. John and Tom are male, while Sarah, Mary, and Ann are female.  
> 1. Define facts for parent-child relationships and individual genders.  
> 2. Write general rules for `father`, `mother`, `sibling` (a person cannot be their own sibling), and `grandparent`.

#### Complete Code
```prolog
/* ==========================================
   Facts: parent(Parent, Child)
   ========================================== */
parent(john, mary).
parent(john, tom).
parent(sarah, mary).
parent(sarah, tom).
parent(mary, ann).

/* ==========================================
   Facts: Genders
   ========================================== */
male(john).
male(tom).
female(sarah).
female(mary).
female(ann).

/* ==========================================
   Rules
   ========================================== */
% A father is a male parent
father(F, C) :- 
    parent(F, C), 
    male(F).

% A mother is a female parent
mother(M, C) :- 
    parent(M, C), 
    female(M).

% Siblings share a common parent and cannot be the same person
sibling(X, Y) :- 
    parent(P, X), 
    parent(P, Y), 
    X \= Y.

% Grandparent is a parent of a parent
grandparent(GP, GC) :- 
    parent(GP, P), 
    parent(P, GC).

% Grandfather / Grandmother specific rules
grandfather(GF, GC) :- 
    grandparent(GF, GC), 
    male(GF).

grandmother(GM, GC) :- 
    grandparent(GM, GC), 
    female(GM).
```

#### Exam Queries & Expected Outputs
```prolog
% 1. Is John the father of Mary?
?- father(john, mary).
% Output: true.

% 2. Who is Ann's mother?
?- mother(M, ann).
% Output: M = mary.

% 3. List all children of John:
?- parent(john, Child).
% Output: Child = mary ; Child = tom.

% 4. Find Mary's siblings:
?- sibling(mary, Sibling).
% Output: Sibling = tom ; Sibling = tom. (Found via father and mother; press Enter to finish)

% 5. Identify who Ann's grandparents are:
?- grandparent(GP, ann).
% Output: GP = john ; GP = sarah.

% 6. Collect all grandparents of Ann into a list (Aggregation):
?- findall(GP, grandparent(GP, ann), List).
% Output: List = [john, sarah].
```

---

### Program 2: Cafe Database & Budget Arithmetic (`cafe.pl`)

#### Problem Statement
> A university cafe sells five items:  
> - Drinks: espresso (\$3), latte (\$5)  
> - Food: croissant (\$4), sandwich (\$8), muffin (\$3)  
> 1. Store item name, category (`food` or `drink`), and price.  
> 2. Rule `affordable(Item, Budget)` for items within budget.  
> 3. Rule `meal_deal(Food, Drink, TotalCost)` combining 1 food + 1 drink and computing sum.

#### Complete Code
```prolog
/* ==========================================
   Facts: item(Name, Category, Price)
   ========================================== */
item(espresso, drink, 3).
item(latte, drink, 5).
item(croissant, food, 4).
item(sandwich, food, 8).
item(muffin, food, 3).

/* ==========================================
   Rules
   ========================================== */
% Check if an item is within a given budget
% CRITICAL: Use '=<', NOT '<='
affordable(Item, Budget) :- 
    item(Item, _, Price), 
    Price =< Budget.

% Combine one food and one drink and calculate total price
% CRITICAL: Use 'is' for arithmetic evaluation
meal_deal(Food, Drink, TotalCost) :- 
    item(Food, food, FoodPrice), 
    item(Drink, drink, DrinkPrice), 
    TotalCost is FoodPrice + DrinkPrice.
```

#### Exam Queries & Expected Outputs
```prolog
% 1. List all food items:
?- item(Food, food, _).
% Output: Food = croissant ; Food = sandwich ; Food = muffin.

% 2. What can you buy with $4 or less?
?- affordable(X, 4).
% Output: X = espresso ; X = croissant ; X = muffin.

% 3. Find every food-and-drink combo for $10 or less:
?- meal_deal(Food, Drink, Total), Total =< 10.
% Output:
% Food = croissant, Drink = espresso, Total = 7 ;
% Food = croissant, Drink = latte,    Total = 9 ;
% Food = muffin,    Drink = espresso, Total = 6 ;
% Food = muffin,    Drink = latte,    Total = 8.
```

---

### Program 3: Course Prerequisites & Recursion (`courses.pl`)

#### Problem Statement
> Requirements:  
> - To enroll in CS102, student must first complete CS101.  
> - CS201 directly requires both CS102 and Math101.  
> - CS301 directly requires CS201.  
> 1. Model direct prerequisites as facts.  
> 2. Rule `can_take(CompletedCourse, TargetCourse)` for direct enrollment.  
> 3. Rule `requires_eventually(Prereq, Target)` checking if a course is required directly OR indirectly through a chain (Recursion / Transitive Closure).

#### Complete Code
```prolog
/* ==========================================
   Facts: prereq(CompletedCourse, TargetCourse)
   ========================================== */
prereq(cs101, cs102).
prereq(cs102, cs201).
prereq(math101, cs201).
prereq(cs201, cs301).

/* ==========================================
   Rules
   ========================================== */
% Direct prerequisite check
can_take(StudentCourse, TargetCourse) :- 
    prereq(StudentCourse, TargetCourse).

% Base Case: Directly required
requires_eventually(Prereq, Target) :- 
    prereq(Prereq, Target).

% Recursive Case: Required through an intermediate course
requires_eventually(Prereq, Target) :- 
    prereq(Prereq, Intermediate), 
    requires_eventually(Intermediate, Target).
```

#### Exam Queries & Expected Outputs
```prolog
% 1. What does CS201 directly require?
?- prereq(Req, cs201).
% Output: Req = cs102 ; Req = math101.

% 2. Can a student jump straight into CS301 from CS101 directly?
?- can_take(cs101, cs301).
% Output: false.

% 3. What are all the foundational courses needed eventually for CS301?
?- requires_eventually(Course, cs301).
% Output: 
% Course = cs201 ;
% Course = cs101 ;
% Course = cs102 ;
% Course = math101.
```

---

### Program 4: Flight Network & Layover Duration (`airport_flights.pl`)

#### Problem Statement
> Airports with cities and countries, plus direct flights with flight numbers and duration in hours.  
> Rules needed: `direct_route/2`, `one_stop_route/3`, `one_stop_duration/3`, and recursive `has_route/2`.

#### Complete Code
```prolog
/* ==========================================
   Facts: airport(City, Country)
   ========================================== */
airport(london, uk).
airport(paris, france).
airport(frankfurt, germany).
airport(dubai, uae).
airport(delhi, india).
airport(singapore, singapore).

/* ==========================================
   Facts: flight(FlightNum, Origin, Destination, DurationHours)
   ========================================== */
flight(ba101, london, paris, 1).
flight(lh202, paris, frankfurt, 1.5).
flight(ek303, frankfurt, dubai, 6).
flight(ek404, london, dubai, 7).
flight(ai505, dubai, delhi, 3.5).
flight(sq606, delhi, singapore, 5.5).

/* ==========================================
   Rules
   ========================================== */
% 1. Direct route exists
direct_route(From, To) :- 
    flight(_, From, To, _).

% 2. Route with exactly one layover
one_stop_route(From, To, Layover) :- 
    flight(_, From, Layover, _), 
    flight(_, Layover, To, _).

% 3. Total duration for a 1-stop route
one_stop_duration(From, To, TotalTime) :- 
    flight(_, From, Layover, T1), 
    flight(_, Layover, To, T2), 
    TotalTime is T1 + T2.

% 4. Recursive Reachability: Any route (direct or multi-stop)
% Base Case
has_route(From, To) :- 
    direct_route(From, To).

% Recursive Case
has_route(From, To) :- 
    direct_route(From, Via), 
    has_route(Via, To).
```

#### Exam Queries & Expected Outputs
```prolog
% 1. Direct flight from London to Paris?
?- direct_route(london, paris).           % true.

% 2. Direct flight from Paris to Delhi?
?- direct_route(paris, delhi).            % false.

% 3. Is it possible to travel from London to Singapore eventually?
?- has_route(london, singapore).          % true.

% 4. 1-stop route from London to Dubai with Layover:
?- one_stop_route(london, dubai, Layover).% Layover = frankfurt (via Paris->Frankfurt->Dubai) or None direct.

% 5. Duration of flight London -> Paris -> Frankfurt:
?- one_stop_duration(london, frankfurt, Hours).
% Output: Hours = 2.5.

% 6. Flights longer than 5 hours:
?- flight(Num, From, To, Duration), Duration > 5.0.

% 7. Negation: Any city reachable from London that does NOT connect to Singapore?
?- direct_route(london, NextCity), \+ has_route(NextCity, singapore).
```

---

### Program 5: Colonel West / Criminal Problem (`criminal.pl`)

#### Problem Statement (Russell & Norvig FOL Inference Classic)
> Narrative:  
> 1. *"It is a crime for an American to sell weapons to hostile nations."*  
> 2. *"The country Nono, an enemy of America, has some missiles."*  
> 3. *"All of its missiles were sold to it by Colonel West."*  
> 4. *"Missiles are weapons."*  
> 5. *"An enemy of America is a hostile nation."*  
> 6. *"Colonel West is American."*  
> Prove: Colonel West is a criminal.

#### Translation Table (First-Order Logic to Prolog Horn Clauses)

| English Sentence | First-Order Logic (FOL) | Prolog Horn Clause |
| :--- | :--- | :--- |
| West is American | $\text{American}(\text{West})$ | `american(west).` |
| Nono is an enemy of America | $\text{Enemy}(\text{Nono}, \text{America})$ | `enemy(nono, america).` |
| $M_1$ is a missile owned by Nono | $\text{Missile}(M_1) \land \text{Owns}(\text{Nono}, M_1)$ | `missile(m1).`<br>`owns(nono, m1).` |
| Missiles are weapons | $\forall x \, (\text{Missile}(x) \implies \text{Weapon}(x))$ | `weapon(X) :- missile(X).` |
| Enemy of America is hostile | $\forall x \, (\text{Enemy}(x, \text{America}) \implies \text{Hostile}(x))$ | `hostile(X) :- enemy(X, america).` |
| West sold all Nono's missiles | $\forall x \, (\text{Missile}(x) \land \text{Owns}(\text{Nono}, x) \implies \text{Sells}(\text{West}, x, \text{Nono}))$ | `sells(west, X, nono) :- missile(X), owns(nono, X).` |
| American selling weapons to hostile nation is criminal | $\forall x, y, z \, (\text{American}(x) \land \text{Weapon}(y) \land \text{Sells}(x, y, z) \land \text{Hostile}(z) \implies \text{Criminal}(x))$ | `criminal(X) :- american(X), weapon(Y), sells(X, Y, Z), hostile(Z).` |

#### Complete Code
```prolog
/* Facts */
american(west).
enemy(nono, america).
missile(m1).
owns(nono, m1).

/* Rules */
weapon(X) :- missile(X).
hostile(X) :- enemy(X, america).
sells(west, X, nono) :- missile(X), owns(nono, X).

criminal(X) :- 
    american(X), 
    weapon(Y), 
    sells(X, Y, Z), 
    hostile(Z).
```

#### Tracing the Proof (`?- trace.`)
To see the step-by-step resolution:
```prolog
?- trace.
?- criminal(west).
```
**Trace Step Walkthrough:**
1. **Call:** `criminal(west)` $\rightarrow$ Prolog attempts to prove West is a criminal.
2. **Call:** `american(west)` $\rightarrow$ Matches fact `american(west)`. (**Exit: Success**)
3. **Call:** `weapon(Y)` $\rightarrow$ Evaluates `weapon(X) :- missile(X)`.
4. **Call:** `missile(Y)` $\rightarrow$ Matches fact `missile(m1)`. (**Exit: `Y = m1`**)
5. **Call:** `sells(west, m1, Z)` $\rightarrow$ Evaluates `sells(west, X, nono) :- missile(X), owns(nono, X)`.
   - Checks `missile(m1)` $\rightarrow$ True.
   - Checks `owns(nono, m1)` $\rightarrow$ True.
   - (**Exit: `Z = nono`**)
6. **Call:** `hostile(nono)` $\rightarrow$ Evaluates `hostile(X) :- enemy(X, america)`.
   - Checks `enemy(nono, america)` $\rightarrow$ True. (**Exit: Success**)
7. **Exit:** `criminal(west)` $\rightarrow$ **true.**

---

## 4. Mastering Recursion (The Universal Template)

Every recursive question in this lab (Ancestors, Prerequisites, Flight routes) follows the exact same **2-clause structure**:

```prolog
% Base Case: Direct 1-step relationship
reach(From, To) :- 
    step(From, To).

% Recursive Case: Take 1 step to an intermediate node, then recurse to destination
reach(From, To) :- 
    step(From, Intermediate), 
    reach(Intermediate, To).
```

### Apply this template to any problem:

| Problem | Base Case | Recursive Case |
| :--- | :--- | :--- |
| **Prerequisites** | `requires_eventually(A, B) :- prereq(A, B).` | `requires_eventually(A, B) :- prereq(A, I), requires_eventually(I, B).` |
| **Flight Routes** | `has_route(A, B) :- direct_route(A, B).` | `has_route(A, B) :- direct_route(A, Via), has_route(Via, B).` |
| **Ancestors** | `ancestor_of(A, B) :- parent_of(A, B).` | `ancestor_of(A, B) :- parent_of(A, Z), ancestor_of(Z, B).` |

> **Memory Trick:** "Direct first, then Step-and-Recurse".

---

## 5. How Prolog Works: Viva & Theory Questions

### Q1. What is the programming paradigm of Prolog?
- **Answer:** **Declarative Programming**. In imperative languages (C++, Java, Python), you tell the computer **how** to solve a problem step-by-step with loops and conditionals. In Prolog, you state **what** the truths and rules are (Knowledge Base), and the language's inference engine solves the queries automatically.

### Q2. What inference method does Prolog use?
- **Answer:** **Backward Chaining** (Goal-driven reasoning) using **SLD-Resolution** (Selective Linear Definite resolution) and **Unification**.
  - Starts with the query as the target goal.
  - Searches top-to-bottom for matching heads of rules or facts.
  - Resolves subgoals left-to-right.

### Q3. How does Backtracking work in Prolog?
- **Answer:** Prolog uses a **Depth-First Search (DFS)** strategy. When an attempt to prove a subgoal fails (or when the user enters `;`), Prolog automatically retreats to the most recent choice point and tries alternative facts or rules.

### Q4. What is a Horn Clause?
- **Answer:** A logical clause with **at most one positive literal**.
  - In FOL: $A \lor \neg B \lor \neg C \equiv (B \land C \implies A)$
  - In Prolog: `A :- B, C.` (`A` is the head/positive literal; `B, C` are negative literals/body conditions).
  - Facts are Horn clauses with no body: `A.`
  - Queries are Horn clauses with no head: `?- B, C.`

### Q5. What is "Negation as Failure" (NAF) and Closed World Assumption (CWA)?
- **Answer:**
  - **Closed World Assumption:** Prolog assumes that anything not declared in its knowledge base or not deducible from its rules is **false**.
  - **Negation as Failure (`\+`):** Prolog tries to prove goal `P`. If `P` fails, `\+ P` succeeds. If `P` succeeds, `\+ P` fails.

### Q6. What are the 4 ports in the Prolog Box Model (Trace)?
- **Call:** Prolog starts trying to prove a goal.
- **Exit:** The goal is successfully proved (variables bound).
- **Redo:** Backtracking occurs; Prolog tries to find an alternative solution for the goal.
- **Fail:** No further solutions exist for the goal.

### Q7. What are the limitations of Prolog compared to Full First-Order Logic?
1. **Horn Clauses Only:** Cannot directly express disjunction in the conclusion ($A \lor B$).
2. **Negation as Failure vs True Logical Negation:** Cannot distinguish between "known to be false" vs "unknown/not stated".
3. **Infinite Loops with Left Recursion:** If a rule is written as `path(X, Y) :- path(X, Z), step(Z, Y).`, Prolog may recurse infinitely before hitting a base case.
4. **Order Dependence:** Rule order (top-to-bottom) and goal order (left-to-right) affect termination and execution order.

---

## 6. Exam Quick-Check & Practice Drill

Before walking into the exam room, ensure you can write these from memory:

- [ ] Write `sibling(X, Y) :- parent(P, X), parent(P, Y), X \= Y.`
- [ ] Write `grandparent(GP, GC) :- parent(GP, P), parent(P, GC).`
- [ ] Write an arithmetic rule using `is` and comparison using `=<`.
- [ ] Write the 2-clause recursive reachability template.
- [ ] Write Colonel West's `criminal/1` rule.
- [ ] Remember to end **every single line** with a period `.`.
- [ ] Remember: **constants are lowercase**, **variables are Uppercase**.
