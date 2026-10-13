# 23CSE401 Fundamentals of AI: Lab 2 Master Study Guide
## First-Order Logic (FOL) up to Inference in SWI-Prolog

> **Notice:** There are **NO CHEAT SHEETS** allowed in the exam. Everything in this guide is structured to help you memorize the syntax patterns, rules, and logic fast.
>
> **🚨 TODAY'S EXAM PATTERN CONFIRMED:** Today's exam for the other class consisted of **Task 1: Extended Family Tree (9 rules, 20 queries a-t)** and **Task 2: Airport Flights Network (4 rules, 20 queries 1-20)**. Both tasks are covered here with every query and answer!

---

## Table of Contents
1. [Core Syntax Rules (The "No Cheat Sheet" Traps)](#1-core-syntax-rules-the-no-cheat-sheet-traps)
2. [SWI-Prolog Console & Shell Operations](#2-swi-prolog-console--shell-operations)
3. [TODAY'S CONFIRMED EXAM PAPER (Task 1 & Task 2)](#3-todays-confirmed-exam-paper-task-1--task-2)
   - [Task 1: Extended Family Tree (Facts, 9 Rules & 20 Queries a-t)](#task-1-extended-family-tree)
   - [Task 2: Airport Flights Network (Facts, 4 Rules & 20 Queries 1-20)](#task-2-airport-flights-network)
4. [ALTERNATE EXAM PAPER (Cafe & Course Sequencing)](#4-alternate-exam-paper)
   - [Problem 1: Cafe Database & Budget Arithmetic (`cafe.pl`)](#problem-1-cafe-database--budget-arithmetic-cafepl)
   - [Problem 2: Course Prerequisites & Transitive Closure (`courses.pl`)](#problem-2-course-prerequisites--transitive-closure-coursespl)
   - [Problem 3: Colonel West / Criminal Problem (`criminal.pl`)](#problem-3-colonel-west--criminal-problem-criminalpl)
5. [Mastering Recursion (The Universal Template)](#5-mastering-recursion-the-universal-template)
6. [How Prolog Works: Viva & Theory Questions](#6-how-prolog-works-viva--theory-questions)
7. [Predicted Exam Questions & Trick Traps](#7-predicted-exam-questions--trick-traps)

---

## 1. Core Syntax Rules (The "No Cheat Sheet" Traps)

| Concept | Syntax | Meaning / Example | Fatal Error to Avoid |
| :--- | :--- | :--- | :--- |
| **Constants / Atoms** | Starts with **lowercase** | `jack`, `helen`, `paris`, `cs101` | Writing `Jack` or `Paris` turns them into variables! |
| **Variables** | Starts with **UPPERCASE** or `_` | `X`, `Person`, `Child`, `Budget` | Writing `x` or `person` treats them as fixed constants. |
| **Rule Operator ("IF")** | `:-` | `Head :- Body.` (Head is true **if** Body is true) | Writing `=:` or `->`. |
| **Conjunction (AND)** | `,` (comma) | `parent_of(X, Y), male(X).` | Using `AND` or `&&`. |
| **Disjunction (OR)** | `;` or multiple clauses | `clause1 ; clause2.` | Writing separate clauses with the same head is cleaner. |
| **Statement Terminator** | `.` (period) | `male(jack).` | **Every fact, rule, and query MUST end with a period.** |
| **Arithmetic Evaluation** | `is` | `Total is T1 + T2.` | **Never use `=` for arithmetic!** `=` is pattern matching. |
| **Less Than or Equal** | `=<` | `Price =< Budget.` | **Do NOT write `<=`.** Prolog only accepts `=<`. |
| **Greater Than or Equal**| `>=` | `Duration >= 5.0.` | Standard `>=`. |
| **Inequality / Not Equal** | `\=` | `X \= Y` | Prevents a person from being their own sibling. |
| **Negation (NOT)** | `\+` | `\+ parent_of(Y, _)` | "Negation as Failure" (cannot be proved from DB). |
| **Anonymous Variable** | `_` (underscore) | `flight(_, From, To, _)` | Use when you don't care about flight number/duration. |

---

## 2. SWI-Prolog Console & Shell Operations

```prolog
?- consult('family.pl').     % Load/consult file (or ?- ['family.pl'].)
?- father_of(jack, X).       % Run query
                             % Press ';' for next answer, Enter to stop
?- listing(father_of).       % View rules/facts for father_of in memory
?- trace.                    % Turn on step-by-step inference debugger
?- notrace.                  % Turn off debugger
?- write('\[2J').         % Clear console screen
?- halt.                     % Exit SWI-Prolog
```

---

## 3. TODAY'S CONFIRMED EXAM PAPER (Task 1 & Task 2)

### Task 1: Extended Family Tree

#### Facts:
```prolog
male(jack).  
male(oliver).  
male(ali).  
male(james).  
male(simon).  
male(harry).  
female(helen).  
female(sophie).  
female(jess).  
female(lily).  

parent_of(jack, jess).  
parent_of(jack, lily).  
parent_of(helen, jess).  
parent_of(helen, lily).  
parent_of(oliver, james).  
parent_of(sophie, james). 
parent_of(jess, simon). 
parent_of(ali, simon). 
parent_of(lily, harry). 
parent_of(james, harry). 
```

#### The 9 Rules:
```prolog
% 1. Father
father_of(X, Y) :- male(X), parent_of(X, Y).

% 2. Mother
mother_of(X, Y) :- female(X), parent_of(X, Y).

% 3. Grandfather
grandfather_of(X, Y) :- male(X), parent_of(X, Z), parent_of(Z, Y).

% 4. Grandmother
grandmother_of(X, Y) :- female(X), parent_of(X, Z), parent_of(Z, Y).

% 5. Sister (female, shared parent, not same person)
sister_of(X, Y) :- female(X), parent_of(P, X), parent_of(P, Y), X \= Y.

% 6. Brother (male, shared parent, not same person)
brother_of(X, Y) :- male(X), parent_of(P, X), parent_of(P, Y), X \= Y.

% 7. Aunt (female, sister of a parent of Y)
aunt_of(X, Y) :- female(X), parent_of(Z, Y), sister_of(X, Z).

% 8. Uncle (male, brother of a parent of Y)
uncle_of(X, Y) :- male(X), parent_of(Z, Y), brother_of(X, Z).

% 9. Ancestor (Recursive transitive closure)
ancestor_of(X, Y) :- parent_of(X, Y).
ancestor_of(X, Y) :- parent_of(X, Z), ancestor_of(Z, Y).
```

#### All 20 Queries & Expected Answers:
| # | Question in Exam | Query Code | Output |
| :--- | :--- | :--- | :--- |
| a | Is Jack the father of Jess? | `?- father_of(jack, jess).` | `true.` |
| b | Is Helen the mother of Lily? | `?- mother_of(helen, lily).` | `true.` |
| c | Are Jess and Lily sisters? | `?- sister_of(jess, lily).` | `true.` |
| d | Is Oliver the grandfather of Harry? | `?- grandfather_of(oliver, harry).` | `true.` |
| e | Is Ali an ancestor of Harry? | `?- ancestor_of(ali, harry).` | `false.` |
| f | Who is the father of Simon? | `?- father_of(X, simon).` | `X = ali.` |
| g | Who is the mother of James? | `?- mother_of(X, james).` | `X = sophie.` |
| h | Who are the children of Lily? | `?- parent_of(lily, Child).` | `Child = harry.` |
| i | Who is the grandmother of Harry? | `?- grandmother_of(X, harry).` | `X = helen ; X = sophie.` |
| j | Who is the uncle of Harry? | `?- uncle_of(X, harry).` | `false.` |
| k | Who is the aunt of Simon? | `?- aunt_of(X, simon).` | `X = lily.` |
| l | Does Harry have any brothers? | `?- brother_of(Brother, harry).` | `false.` |
| m | Find all father-child pairs: | `?- father_of(Father, Child).` | Binds all pairs |
| n | Find all sisters in the family: | `?- sister_of(Sister, Sibling).` | Binds all sisters |
| o | Descendants of Helen: | `?- ancestor_of(helen, Descendant).` | `jess, lily, simon, harry` |
| p | Ancestors of Simon: | `?- ancestor_of(Ancestor, simon).` | `jess, ali, jack, helen` |
| q | All grandfathers and grandchildren: | `?- grandfather_of(GF, GC).` | Binds all pairs |
| r | Males who are also fathers: | `?- male(X), father_of(X, _).` | `jack, oliver, ali, james` |
| s | Has both Jack & Helen as parents: | `?- parent_of(jack, X), parent_of(helen, X).` | `X = jess ; X = lily.` |
| t | Parent with NO grandchildren: | `?- parent_of(X, Y), \+ parent_of(Y, _).` | `ali, lily, james` |

---

### Task 2: Airport Flights Network

#### Facts:
```prolog
airport(london, uk).  
airport(paris, france).  
airport(frankfurt, germany).  
airport(dubai, uae).  
airport(delhi, india).  
airport(singapore, singapore). 

flight(ba101, london, paris, 1). 
flight(lh202, paris, frankfurt, 1.5). 
flight(ek303, frankfurt, dubai, 6). 
flight(ek404, london, dubai, 7). 
flight(ai505, dubai, delhi, 3.5). 
flight(sq606, delhi, singapore, 5.5). 
```

#### The 4 Rules:
```prolog
% 1. Direct route
direct_route(From, To) :-  
    flight(_, From, To, _). 

% 2. Exactly one layover
one_stop_route(From, To, Layover) :- 
    flight(_, From, Layover, _), 
    flight(_, Layover, To, _). 

% 3. Total duration for 1-stop
one_stop_duration(From, To, TotalTime) :- 
    flight(_, From, Layover, Time1), 
    flight(_, Layover, To, Time2), 
    TotalTime is Time1 + Time2. 

% 4. Recursive reachability (Direct or multi-stop)
has_route(From, To) :-  
    direct_route(From, To). 
has_route(From, To) :-  
    direct_route(From, Via),  
    has_route(Via, To). 
```

#### All 20 Queries & Expected Answers:
| # | Exam Question | Query Code | Output |
| :--- | :--- | :--- | :--- |
| 1 | Direct flight London &rarr; Paris? | `?- direct_route(london, paris).` | `true.` |
| 2 | Direct flight Paris &rarr; Delhi? | `?- direct_route(paris, delhi).` | `false.` |
| 3 | London &rarr; Singapore eventually? | `?- has_route(london, singapore).` | `true.` |
| 4 | Where to fly directly from London? | `?- direct_route(london, Destination).` | `paris ; dubai.` |
| 5 | Cities with direct flight into Dubai? | `?- direct_route(Origin, dubai).` | `frankfurt ; london.` |
| 6 | Flight number leaving Delhi? | `?- flight(FlightNum, delhi, _, _).` | `FlightNum = sq606.` |
| 7 | 1-stop London &rarr; Dubai layover city? | `?- one_stop_route(london, dubai, Layover).` | `false.` (Direct or 2-stop only!) |
| 8 | Destination from Frankfurt with 1 stop? | `?- one_stop_route(frankfurt, Destination, _).` | `Destination = delhi.` |
| 9 | All direct routes and durations: | `?- flight(_, Origin, Destination, Duration).` | Lists all 6 |
| 10 | Duration London &rarr; Paris &rarr; Frankfurt? | `?- one_stop_duration(london, frankfurt, Hours).` | `Hours = 2.5.` |
| 11 | 1-stop London &rarr; Delhi + total time: | `?- one_stop_route(london, delhi, L), one_stop_duration(london, delhi, T).` | `L = dubai, T = 10.5.` |
| 12 | All pairs connected by 1-stop flight: | `?- one_stop_route(CityA, CityB, _).` | Lists pairs |
| 13 | Flights longer than 5 hours: | `?- flight(Num, From, To, Duration), Duration > 5.0.` | `ek303 (6), ek404 (7), sq606 (5.5)` |
| 14 | Short flights under 2 hours: | `?- flight(Num, From, To, Duration), Duration < 2.0.` | `ba101 (1), lh202 (1.5)` |
| 15 | Flights where destination country is UK? | `?- flight(_, _, Dest, _), airport(Dest, uk).` | `false.` |
| 16 | Flight departing from city in France: | `?- airport(City, france), flight(_, City, Dest, _).` | `City = paris, Dest = frankfurt.` |
| 17 | All destinations reachable from London: | `?- has_route(london, Destination).` | `paris, frankfurt, dubai, delhi, singapore` |
| 18 | Flight number starting with 'ek': | `?- flight(FlightNum, _, _, _), sub_atom(FlightNum, 0, 2, _, ek).` | `ek303 ; ek404.` |
| 19 | 1-stop destination from Paris taking 7.5h: | `?- one_stop_duration(paris, Dest, 7.5).` | `Dest = dubai.` |
| 20 | City from London not connecting to Singapore: | `?- direct_route(london, C), \+ has_route(C, singapore).` | `false.` |

---

## 4. ALTERNATE EXAM PAPER

### Problem 1: Cafe Database & Budget Arithmetic (`cafe.pl`)
```prolog
item(espresso, drink, 3).
item(latte, drink, 5).
item(croissant, food, 4).
item(sandwich, food, 8).
item(muffin, food, 3).

% Note '=<' NOT '<='
affordable(Item, Budget) :- 
    item(Item, _, Price), 
    Price =< Budget.

% Note 'is' for math
meal_deal(Food, Drink, TotalCost) :- 
    item(Food, food, FoodPrice), 
    item(Drink, drink, DrinkPrice), 
    TotalCost is FoodPrice + DrinkPrice.
```

### Problem 2: Course Prerequisites & Transitive Closure (`courses.pl`)
```prolog
prereq(cs101, cs102).
prereq(cs102, cs201).
prereq(math101, cs201).
prereq(cs201, cs301).

can_take(StudentCourse, TargetCourse) :- 
    prereq(StudentCourse, TargetCourse).

% Base case
requires_eventually(Prereq, Target) :- 
    prereq(Prereq, Target).
% Recursive case
requires_eventually(Prereq, Target) :- 
    prereq(Prereq, Intermediate), 
    requires_eventually(Intermediate, Target).
```

### Problem 3: Colonel West / Criminal Problem (`criminal.pl`)
```prolog
american(west).
enemy(nono, america).
missile(m1).
owns(nono, m1).

weapon(X) :- missile(X).
hostile(X) :- enemy(X, america).
sells(west, X, nono) :- missile(X), owns(nono, X).

criminal(X) :- 
    american(X), 
    weapon(Y), 
    sells(X, Y, Z), 
    hostile(Z).
```

---

## 5. Mastering Recursion (The Universal Template)

```prolog
% Base Case: Direct 1-step relationship
reach(From, To) :- 
    step(From, To).

% Recursive Case: Step to intermediate, then recurse
reach(From, To) :- 
    step(From, Intermediate), 
    reach(Intermediate, To).
```

---

## 6. How Prolog Works: Viva & Theory Questions

- **Paradigm:** Declarative (states what relations hold, not how to compute).
- **Inference:** Backward chaining, SLD-resolution, Unification.
- **Search Strategy:** Depth-First Search (DFS) with automatic backtracking.
- **Horn Clause:** Clause with at most 1 positive literal: `Head :- Body.`
- **Closed World Assumption:** Anything not provable from facts is assumed false (`\+`).
- **4 Trace Ports:** `Call` (try to prove), `Exit` (proved/bound), `Redo` (backtrack for next), `Fail` (cannot prove).

---

## 7. Predicted Exam Questions & Trick Traps

1. **Parent with no grandkids:** `parent_of(X, Y), \+ parent_of(Y, _).`
2. **Substring check:** `sub_atom(FlightNum, 0, 2, _, ek).`
3. **Exact duration match:** `one_stop_duration(paris, Dest, 7.5).`
4. **Left-recursion crash:** Writing `reach(A, B) :- reach(A, I), step(I, B).` causes infinite loop! Always put concrete `step(A, I)` first!
