# 23CSE401 Fundamentals of AI: Lab 2 Master Study Guide
## First-Order Logic (FOL) up to Inference in SWI-Prolog

> **⚠️ EXAM NOTICE:** There are **NO CHEAT SHEETS** allowed in the exam room. Everything in this guide is structured to help you memorize the syntax patterns, rules, and logic fast.
>
> **Confirmed Exam Coverage:**
> 1. 📚 **Syllabus & Lecture Presentation** (`prologue.pptx` & `Role of Prologue.pdf` – `likes.pl`, Bratko `family.pl`, 3 Pillars of Symbolic AI)
> 2. 🚨 **Set 1 (CSE Section G)** (`23cse401 Lab Evaluation1-set1_cse G.pdf` – Room layout, 4-Color map CSP, Europe geographic math)
> 3. 🔥 **Family & Flights** (`Logic and Prolog 2.pdf` / `Logic and Prolog copy.docx` – 10-person family tree with 9 rules & 20 queries a–t, Airport flights with 4 rules & 20 queries 1–20)
> 4. ☕ **Cafe & Prereqs** (`23cse401 FoAI Lab Ex.pdf` / `Logic and Prolog.docx` – Cafe budget arithmetic, Course prerequisites transitive closure)
> 5. 🎖️ **Colonel West** (`criminal.pl` – FOL Criminal problem translation & backward chaining)
> 6. 📖 **Syntax & Viva** (The 5 fatal traps, universal recursion template, 4 trace ports, viva Q&A)

---

## Table of Contents
1. [📚 Module 1: Syllabus & Presentation Foundations (`prologue.pptx` & `Role of Prologue.pdf`)](#-module-1-syllabus--presentation-foundations)
   - [1.1 The 3 Core Pillars of Symbolic AI (`Role of Prologue.pdf`)](#11-the-3-core-pillars-of-symbolic-ai)
   - [1.2 Presentation Exercise 1: `likes.pl` (Slide 3–5)](#12-presentation-exercise-1-likespl)
   - [1.3 Presentation Exercise 2: Bratko Introductory Family Tree `family.pl` (Slide 8–10)](#13-presentation-exercise-2-introductory-family-tree-familypl)
   - [1.4 SWI-Prolog Getting Started Workflow & Shell Commands](#14-swi-prolog-getting-started-workflow--shell-commands)
2. [🚨 Module 2: Official Lab Evaluation 1 - Set 1 (CSE Section G)](#-module-2-official-lab-evaluation-1---set-1-cse-section-g)
   - [2.1 Problem 1: A Small World (Room Knowledge Representation & Spatial Rules)](#21-problem-1-a-small-world-room-knowledge-representation)
   - [2.2 Problem 2: The 4-Color Map Problem (Constraint Satisfaction)](#22-problem-2-the-4-color-map-problem-csp)
   - [2.3 Problem 3: Europe Geographic Database (Density Math & Neighbors)](#23-problem-3-europe-geographic-database)
3. [🔥 Module 3: Confirmed Exam Paper - Family Tree & Airport Flights](#-module-3-confirmed-exam-paper---family-tree--airport-flights)
   - [3.1 Task 1: Extended 10-Person Family Tree (Facts, 9 Rules & All 20 Queries a–t)](#31-task-1-extended-10-person-family-tree)
   - [3.2 Task 2: Airport Flights Network (Facts, 4 Rules & All 20 Queries 1–20)](#32-task-2-airport-flights-network)
4. [☕ Module 4: Alternate Exam Paper - Cafe & Course Prerequisites](#-module-4-alternate-exam-paper---cafe--course-prerequisites)
   - [4.1 Problem 1: Campus Cafe Database & Budget Arithmetic (`cafe.pl`)](#41-problem-1-campus-cafe-database--budget-arithmetic-cafepl)
   - [4.2 Problem 2: Course Prerequisites & Transitive Closure (`courses.pl`)](#42-problem-2-course-prerequisites--transitive-closure-coursespl)
5. [🎖️ Module 5: Colonel West / The Criminal Problem (`criminal.pl`)](#-module-5-colonel-west--the-criminal-problem)
   - [5.1 Russell & Norvig FOL Criminal Problem Translation](#51-russell--norvig-fol-criminal-problem-translation)
   - [5.2 Step-by-Step Backward Chaining Proof Trace](#52-step-by-step-backward-chaining-proof-trace)
6. [📖 Module 6: Syntax Traps, Universal Recursion & Viva Preparation](#-module-6-syntax-traps-universal-recursion--viva-preparation)
   - [6.1 The 5 Fatal "No Cheat Sheet" Traps](#61-the-5-fatal-no-cheat-sheet-traps)
   - [6.2 The Universal Recursion Template & Left-Recursion Infinite Loop Trap](#62-the-universal-recursion-template)
   - [6.3 4-Port Debugger Model (`Call`, `Exit`, `Redo`, `Fail`)](#63-4-port-debugger-model)
   - [6.4 Viva & Theory Oral Exam Master Q&A](#64-viva--theory-oral-exam-master-qa)

---

## 📚 Module 1: Syllabus & Presentation Foundations

> **Source Documents:** `prologue.pptx` (Course Lecture Slides) & `Role of Prologue.pdf` (Curriculum Rationale)

### 1.1 The 3 Core Pillars of Symbolic AI

In modern AI, deep learning uses statistical approximations. In contrast, Prolog represents **Classical Symbolic AI** and **Knowledge-Based Reasoning**:

1. **Shift to Declarative Programming:**
   - **Imperative (C++, Java, Python):** You explicitly instruct the computer *how* to compute results step-by-step using loops and variable reassignments.
   - **Declarative (Prolog):** You state *what* is true by defining **Facts** (unconditional assertions) and **Rules** (conditional implications). The execution engine searches for truth automatically.

2. **Hands-on Knowledge Representation:**
   - Translates natural human propositions into **First-Order Predicate Logic** and **Horn Clauses**.
   - Proposition: *"Every human is mortal"* &rarr; First-Order Logic: `∀X human(X) ⇒ mortal(X)` &rarr; Prolog Rule: `mortal(X) :- human(X).`

3. **Automated Inference & Backtracking:**
   - Uses **Resolution Refutation** and **Unification** (pattern matching of terms and variables).
   - Solves queries via **Backward Chaining** driven by a built-in **Depth-First Search (DFS)** algorithm across the logical tree.
   - When a branch fails or the user requests another answer (`;`), Prolog **backtracks** to the most recent choice point to explore alternate solutions.

---

### 1.2 Presentation Exercise 1: `likes.pl`

*(From Slide 3, Slide 4, and Slide 5 of `prologue.pptx`)*

#### Program Code (`likes.pl`):
```prolog
% Unconditional Facts
likes(john, pizza).
likes(john, pasta).
likes(mary, sushi).
likes(mary, pizza).
likes(john, mary).
likes(mary, jay).

% Slide 3 Transitive Rule: X likes Y if X likes Z and Z likes Y
likes(X, Y) :- 
    likes(X, Z), 
    likes(Z, Y).

% Slide 3 Family Rule: Grandfather
grandfather(X, Y) :- 
    father(X, Z), 
    parent(Z, Y).
```

#### Console Operations & Queries from Slides:
```prolog
% 1. Check if the file is loaded into the active runtime:
?- source_file('likes.pl').

% 2. View all currently loaded clauses for the predicate:
?- listing(likes).

% 3. What does Mary like? (Variables MUST be uppercase)
?- likes(mary, X).
% Output:
% X = sushi ;
% X = pizza ;
% X = jay.

% 4. Who likes pizza?
?- likes(Y, pizza).
% Output:
% Y = john ;
% Y = mary.

% 5. Clear the SWI-Prolog console screen (Slide 3 & 11):
?- write('\33[2J').

% 6. User interaction keys during query evaluation:
% Press ';' or 'n' or 'Space' -> Ask Prolog for the NEXT solution
% Press 'Enter'              -> Accept current solution and STOP
% Press 'Ctrl+C' then 'a'    -> ABORT long/infinite query execution
```

---

### 1.3 Presentation Exercise 2: Introductory Family Tree (`family.pl`)

*(From Slide 8, 9, and 10 of `prologue.pptx` – Ivan Bratko Figure 1.1 benchmark)*

#### Program Code (`family.pl`):
```prolog
% 6 parent facts defining the introductory family tree
parent(pam, bob).
parent(tom, bob).
parent(tom, liz).
parent(bob, ann).
parent(bob, pat).
parent(pat, jim).
```

#### Slide Queries & Explanations:
```prolog
% Query 1: Is Bob a parent of Pat?
?- parent(bob, pat).
% Output: true. (Direct fact match)

% Query 2: Who is the parent of Liz?
?- parent(X, liz).
% Output: X = tom. (Unification binds variable X to atom tom)

% Query 3: Who are all the children of Bob?
?- parent(bob, Child).
% Output: 
% Child = ann ;
% Child = pat.

% Query 4: Inspect predicate clauses:
?- listing(parent).
```

---

### 1.4 SWI-Prolog Getting Started Workflow & Shell Commands

| Step | Action | Command / Shortcut |
| :--- | :--- | :--- |
| **1. Create Code** | Save code in plain text | Save as `filename.pl` (e.g. `likes.pl`) |
| **2. Launch SWI** | Terminal / Shell | Type `swipl` and press Enter |
| **3. Load File** | Consult into memory | `?- consult('filename.pl').` or `?- ['filename.pl'].` |
| **4. Check Load** | Verify file status | `?- source_file('filename.pl').` |
| **5. Inspect KB** | Print predicate definitions | `?- listing(predicate_name).` |
| **6. Turn on Debug** | Enable 4-port trace | `?- trace.` (Press Enter through steps) |
| **7. Turn off Debug**| Disable trace | `?- notrace.` |
| **8. Clear Screen** | ANSI escape sequence | `?- write('\33[2J').` |
| **9. Exit Runtime** | Quit SWI-Prolog | `?- halt.` |

---

## 🚨 Module 2: Official Lab Evaluation 1 - Set 1 (CSE Section G)

> **Paper Reference:** `23cse401 Lab Evaluation1-set1_cse G.pdf` (Nosco AI Benchmark)

### 2.1 Problem 1: A Small World (Room Knowledge Representation)

You are asked to model the furniture and structure of a room using facts, then answer spatial relationships.

#### Facts:
```prolog
furniture(wardrobe, 1).
furniture(bookshelf, 1).
furniture(bed, 1).
furniture(desk, 1).
furniture(chair, 1).

number(door, 1).
number(window, 2).

at(wall_1, wardrobe).
at(wall_1, door).
at(wall_2, bookshelf).
at(wall_2, bed).
at(wall_3, bed).
at(wall_3, window_1).
at(wall_3, desk).
at(wall_4, desk).
at(wall_4, window_2).
at(wall_4, wardrobe).
at(window_1, bed).
at(window_2, desk).

in(corner_1, wardrobe).
in(corner_3, bed).
in(corner_4, desk).

left(door, bookshelf).
left(bookshelf, bed).
left(bed, desk).
left(desk, wardrobe).
left(wardrobe, door).
```

#### Rules:
```prolog
% 1. Which furniture is in the room?
furniture(X) :- furniture(X, N).

% 2. How many items of an object exist in the room?
number(X, N) :- furniture(X, N).

% 3. Where is an object placed? (Checks at-wall or in-corner)
place(X, P) :- at(P, X).
place(X, P) :- in(P, X).
place(chair, at_desk).

% 4. Left and Right relations (Right is the symmetric inverse of Left)
right(X, Y) :- left(Y, X).
```

#### Queries & Outputs:
| Goal | Query Code | Result |
| :--- | :--- | :--- |
| Which furniture is in the room? | `?- furniture(X).` | `wardrobe`, `bookshelf`, `bed`, `desk`, `chair` |
| How many doors in the room? | `?- number(door, N).` | `N = 1.` |
| Where is the chair placed? | `?- place(chair, P).` | `P = at_desk.` |
| Is the bookshelf to the left of the bed? | `?- left(bookshelf, bed).` | `true.` |
| What is to the right of the bed? | `?- right(X, bed).` | `X = bookshelf.` |
| What object is at wall 2? | `?- at(wall_2, Obj).` | `Obj = bookshelf ; Obj = bed.` |
| What object is in corner 1? | `?- in(corner_1, Obj).`| `Obj = wardrobe.` |

---

### 2.2 Problem 2: The 4-Color Map Problem (CSP)

Given 5 regions `A`, `B`, `C`, `D`, `E` on a planar map, assign colors (`red`, `blue`, `yellow`, `green`) such that no two adjacent regions share the same color.

#### Program Code:
```prolog
color(red).
color(blue).
color(yellow).
color(green).

% Adjacency rule: Two regions have valid colors if they are different colors
adjacent(X, Y) :- 
    color(X), 
    color(Y), 
    X \= Y.

% Map constraint solver: Every border must satisfy adjacent/2
map(A, B, C, D, E) :-
    adjacent(A, B), adjacent(A, D), adjacent(A, E),
    adjacent(B, C), adjacent(B, D), adjacent(B, E),
    adjacent(C, D), adjacent(C, E),
    adjacent(D, E).
```

#### Query:
```prolog
?- map(A, B, C, D, E).
% Output:
% A = red, B = blue, C = red, D = yellow, E = green ;
% (Prolog backtracks to generate all valid non-conflicting colorings)
```

---

### 2.3 Problem 3: Europe Geographic Database

A geographic knowledge base recording area (km²), population, and shared borders for European states.

#### Facts:
```prolog
area(austria, 83858).
area(france, 547030).
area(germany, 357021).
area(italy, 301230).
area(liechtenstein, 160). 
area(spain, 504851).
area(switzerland, 41290).
area(united_kingdom, 244820).

population(austria, 8169929).
population(france, 63182000).
population(germany, 83251851).
population(italy, 59530464).
population(liechtenstein, 32842). 
population(spain, 47059533).
population(switzerland, 7507000).
population(united_kingdom, 61100835).

neighbor(austria, switzerland).
neighbor(france, switzerland).
neighbor(france, germany).
neighbor(france, spain).
neighbor(germany, switzerland).
neighbor(italy, switzerland).
neighbor(liechtenstein, switzerland).
```

#### Rules & Queries:
```prolog
% 1. Population Density: D = population / area (MUST use 'is' for arithmetic evaluation!)
density(S, D) :- 
    population(S, Pop), 
    area(S, Area), 
    D is Pop / Area.

% 2. Bidirectional Neighbor: Borders are reciprocal
neighbors(S1, S2) :- neighbor(S1, S2).
neighbors(S1, S2) :- neighbor(S2, S1).

% Query 1: Which states border Switzerland?
neighbor_ch(S) :- neighbors(switzerland, S).
% Output: austria, france, germany, italy, liechtenstein.

% Query 2: Which states border France?
neighbor_f(S) :- neighbors(france, S).
% Output: switzerland, germany, spain.

% Query 3 (TRICK QUESTION): State bordering Germany with BOTH larger population AND larger area?
neighbor_d(S) :- 
    neighbors(germany, S), 
    population(germany, PopGer), population(S, PopS),
    area(germany, AreaGer), area(S, AreaS), 
    PopS > PopGer, 
    AreaS > AreaGer.
% Output: false. 
% EXAM EXPLANATION: France borders Germany and has larger area (547,030 > 357,021), 
% but smaller population (63,182,000 < 83,251,851). No country satisfies BOTH conditions!

% Query 4: Neighbors of Switzerland with higher density than Switzerland?
neighbor_ch1(S) :- 
    neighbors(switzerland, S),
    density(switzerland, DCH), 
    density(S, DS), 
    DS > DCH.
% Output: germany (233.18), italy (197.62), liechtenstein (205.26).
```

---

## 🔥 Module 3: Confirmed Exam Paper - Family Tree & Airport Flights

> **Source Documents:** `Logic and Prolog 2.pdf` & `Logic and Prolog copy.docx`

### 3.1 Task 1: Extended 10-Person Family Tree

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

#### The 9 Required Rules:
```prolog
% 1. Father: male parent
father_of(X, Y) :- male(X), parent_of(X, Y).

% 2. Mother: female parent
mother_of(X, Y) :- female(X), parent_of(X, Y).

% 3. Grandfather: male parent of a parent
grandfather_of(X, Y) :- male(X), parent_of(X, Z), parent_of(Z, Y).

% 4. Grandmother: female parent of a parent
grandmother_of(X, Y) :- female(X), parent_of(X, Z), parent_of(Z, Y).

% 5. Sister: female, shares parent P with Y, not the same person (X \= Y)
sister_of(X, Y) :- female(X), parent_of(P, X), parent_of(P, Y), X \= Y.

% 6. Brother: male, shares parent P with Y, not the same person (X \= Y)
brother_of(X, Y) :- male(X), parent_of(P, X), parent_of(P, Y), X \= Y.

% 7. Aunt: female, sister of parent Z of Y
aunt_of(X, Y) :- female(X), parent_of(Z, Y), sister_of(X, Z).

% 8. Uncle: male, brother of parent Z of Y
uncle_of(X, Y) :- male(X), parent_of(Z, Y), brother_of(X, Z).

% 9. Ancestor: direct parent OR parent of an ancestor (transitive closure)
ancestor_of(X, Y) :- parent_of(X, Y).
ancestor_of(X, Y) :- parent_of(X, Z), ancestor_of(Z, Y).
```

#### All 20 Queries (a through t) & Verified Answers:
| # | Question in Exam | Exact Query Code | Expected Output |
| :--- | :--- | :--- | :--- |
| **a** | Is Jack the father of Jess? | `?- father_of(jack, jess).` | `true.` |
| **b** | Is Helen the mother of Lily? | `?- mother_of(helen, lily).` | `true.` |
| **c** | Are Jess and Lily sisters? | `?- sister_of(jess, lily).` | `true.` |
| **d** | Is Oliver the grandfather of Harry? | `?- grandfather_of(oliver, harry).` | `true.` |
| **e** | Is Ali an ancestor of Harry? | `?- ancestor_of(ali, harry).` | `false.` |
| **f** | Who is the father of Simon? | `?- father_of(X, simon).` | `X = ali.` |
| **g** | Who is the mother of James? | `?- mother_of(X, james).` | `X = sophie.` |
| **h** | Who are the children of Lily? | `?- parent_of(lily, Child).` | `Child = harry.` |
| **i** | Who is the grandmother of Harry? | `?- grandmother_of(X, harry).` | `X = helen ; X = sophie.` |
| **j** | Who is the uncle of Harry? | `?- uncle_of(X, harry).` | `false.` |
| **k** | Who is the aunt of Simon? | `?- aunt_of(X, simon).` | `X = lily.` |
| **l** | Does Harry have any brothers? | `?- brother_of(Brother, harry).` | `false.` |
| **m** | Find all father-child pairs: | `?- father_of(Father, Child).` | Lists all 5 father relationships |
| **n** | Find all sisters in the family: | `?- sister_of(Sister, Sibling).` | `Sister = jess, Sibling = lily ; Sister = lily, Sibling = jess.` |
| **o** | Find all descendants of Helen: | `?- ancestor_of(helen, Descendant).` | `Descendant = jess ; lily ; simon ; harry.` |
| **p** | Find all ancestors of Simon: | `?- ancestor_of(Ancestor, simon).` | `Ancestor = jess ; ali ; jack ; helen.` |
| **q** | All grandfathers and grandchildren: | `?- grandfather_of(GF, GC).` | `GF = jack, GC = simon ; GF = jack, GC = harry ; GF = oliver, GC = harry.` |
| **r** | Males who are also fathers: | `?- male(X), father_of(X, _).` | `X = jack ; oliver ; ali ; james.` |
| **s** | Has both Jack & Helen as parents: | `?- parent_of(jack, X), parent_of(helen, X).` | `X = jess ; X = lily.` |
| **t** | Parent who has NO grandchildren: | `?- parent_of(X, Y), \+ parent_of(Y, _).` | `X = ali ; lily ; james.` |

---

### 3.2 Task 2: Airport Flights Network

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

#### The 4 Required Rules:
```prolog
% 1. Direct route exists between two cities
direct_route(From, To) :-  
    flight(_, From, To, _). 

% 2. Exactly one layover exists
one_stop_route(From, To, Layover) :- 
    flight(_, From, Layover, _), 
    flight(_, Layover, To, _). 

% 3. Total flight duration for a 1-stop trip (MUST use 'is' for addition!)
one_stop_duration(From, To, TotalTime) :- 
    flight(_, From, Layover, Time1), 
    flight(_, Layover, To, Time2), 
    TotalTime is Time1 + Time2. 

% 4. Recursive reachability (Direct flight OR step through intermediate city)
has_route(From, To) :-  
    direct_route(From, To). 
has_route(From, To) :-  
    direct_route(From, Via),  
    has_route(Via, To). 
```

#### All 20 Queries (1 through 20) & Verified Answers:
| # | Question in Exam | Exact Query Code | Expected Output |
| :--- | :--- | :--- | :--- |
| **1** | Direct flight London &rarr; Paris? | `?- direct_route(london, paris).` | `true.` |
| **2** | Direct flight Paris &rarr; Delhi? | `?- direct_route(paris, delhi).` | `false.` |
| **3** | Can a passenger travel London &rarr; Singapore? | `?- has_route(london, singapore).` | `true.` |
| **4** | Where can one fly directly from London? | `?- direct_route(london, Destination).` | `Destination = paris ; Destination = dubai.` |
| **5** | Cities with a direct flight arriving in Dubai? | `?- direct_route(Origin, dubai).` | `Origin = frankfurt ; Origin = london.` |
| **6** | What is the flight number departing Delhi? | `?- flight(FlightNum, delhi, _, _).` | `FlightNum = sq606.` |
| **7** | Layover city for a 1-stop flight London &rarr; Dubai? | `?- one_stop_route(london, dubai, Layover).` | `false.` *(London &rarr; Dubai is direct or 2 stops via Frankfurt)* |
| **8** | Reachable from Frankfurt with exactly 1 layover? | `?- one_stop_route(frankfurt, Destination, _).` | `Destination = delhi.` *(via Dubai)* |
| **9** | List all direct flights and durations: | `?- flight(_, Origin, Destination, Duration).` | Lists all 6 flights |
| **10**| Total travel time London &rarr; Paris &rarr; Frankfurt? | `?- one_stop_duration(london, frankfurt, Hours).` | `Hours = 2.5.` *(1 + 1.5)* |
| **11**| 1-stop London &rarr; Delhi: layover & total time? | `?- one_stop_route(london, delhi, L), one_stop_duration(london, delhi, T).` | `L = dubai, T = 10.5.` |
| **12**| Find all city pairs connected by a 1-stop flight: | `?- one_stop_route(CityA, CityB, _).` | `(london, frankfurt), (paris, dubai), (frankfurt, delhi), (london, delhi), (dubai, singapore)` |
| **13**| Flights strictly longer than 5 hours: | `?- flight(Num, From, To, Duration), Duration > 5.0.` | `ek303 (6h), ek404 (7h), sq606 (5.5h)` |
| **14**| Short flights under 2 hours: | `?- flight(Num, From, To, Duration), Duration < 2.0.` | `ba101 (1h), lh202 (1.5h)` |
| **15**| Flights where destination country is the UK? | `?- flight(_, _, Dest, _), airport(Dest, uk).` | `false.` *(No flights arrive into London)* |
| **16**| Flight departing from a city in France: | `?- airport(City, france), flight(_, City, Dest, _).` | `City = paris, Dest = frankfurt.` |
| **17**| All destinations reachable from London: | `?- has_route(london, Destination).` | `paris, frankfurt, dubai, delhi, singapore` |
| **18**| Flights whose flight number starts with 'ek': | `?- flight(FlightNum, _, _, _), sub_atom(FlightNum, 0, 2, _, ek).` | `FlightNum = ek303 ; FlightNum = ek404.` |
| **19**| 1-stop destination from Paris taking exactly 7.5h? | `?- one_stop_duration(paris, Dest, 7.5).` | `Dest = dubai.` *(1.5 + 6.0)* |
| **20**| City directly reachable from London with NO route to Singapore? | `?- direct_route(london, C), \+ has_route(C, singapore).` | `false.` *(Both Paris and Dubai reach Singapore!)* |

---

## ☕ Module 4: Alternate Exam Paper - Cafe & Course Prerequisites

> **Source Documents:** `23cse401 FoAI Lab Ex.pdf` & `Logic and Prolog.docx`

### 4.1 Problem 1: Campus Cafe Database & Budget Arithmetic (`cafe.pl`)

#### Facts:
```prolog
item(espresso, drink, 3).
item(latte, drink, 5).
item(cappuccino, drink, 4).
item(croissant, food, 4).
item(sandwich, food, 8).
item(muffin, food, 3).
```

#### Rules:
```prolog
% Affordable rule: Price is less than or equal to Budget
% FATAL TRAP: Use '=<' and NOT '<='
affordable(Item, Budget) :- 
    item(Item, _, Price), 
    Price =< Budget.

% Meal deal rule: Combines 1 food and 1 drink within total budget
% FATAL TRAP: Use 'is' and NOT '=' for arithmetic addition
meal_deal(Food, Drink, TotalCost) :- 
    item(Food, food, FoodPrice), 
    item(Drink, drink, DrinkPrice), 
    TotalCost is FoodPrice + DrinkPrice.

affordable_meal_deal(Food, Drink, Budget, TotalCost) :-
    meal_deal(Food, Drink, TotalCost),
    TotalCost =< Budget.
```

#### Exam Queries:
```prolog
% Find all drinks affordable with 4 dollars:
?- item(Item, drink, Price), Price =< 4.
% Output: espresso ($3), cappuccino ($4).

% Can you get a sandwich and latte for under 12 dollars?
?- meal_deal(sandwich, latte, Total), Total =< 12.
% Output: false. (8 + 5 = 13 > 12)
```

---

### 4.2 Problem 2: Course Prerequisites & Transitive Closure (`courses.pl`)

#### Facts:
```prolog
prereq(cs101, cs102).
prereq(cs102, cs201).
prereq(math101, cs201).
prereq(cs201, cs301).
```

#### Rules:
```prolog
% Direct prerequisite:
can_take(StudentCourse, TargetCourse) :- 
    prereq(StudentCourse, TargetCourse).

% Transitive closure: Requires eventually
% Base case: Direct requirement
requires_eventually(Prereq, Target) :- 
    prereq(Prereq, Target).

% Recursive case: Step to Intermediate, then recurse
requires_eventually(Prereq, Target) :- 
    prereq(Prereq, Intermediate), 
    requires_eventually(Intermediate, Target).
```

#### Exam Queries:
```prolog
% Does cs301 eventually require cs101?
?- requires_eventually(cs101, cs301).
% Output: true. (cs101 -> cs102 -> cs201 -> cs301)

% What are all courses that math101 is needed for?
?- requires_eventually(math101, Course).
% Output: Course = cs201 ; Course = cs301.
```

---

## 🎖️ Module 5: Colonel West / The Criminal Problem

> **Source:** Russell & Norvig First-Order Logic Inference Benchmark (`criminal.pl`)

### 5.1 Russell & Norvig FOL Criminal Problem Translation

Natural language problem:
> *"The law says that it is a crime for an American to sell weapons to hostile nations. The country Nono, an enemy of America, has some missiles, and all of its missiles were sold to it by Colonel West, who is American."*

#### Prolog Knowledge Base:
```prolog
% Facts:
american(west).
enemy(nono, america).
missile(m1).
owns(nono, m1).

% Rules translated from FOL Horn Clauses:
weapon(X) :- missile(X).
hostile(X) :- enemy(X, america).
sells(west, X, nono) :- missile(X), owns(nono, X).

% Crime Definition:
criminal(X) :- 
    american(X), 
    weapon(Y), 
    sells(X, Y, Z), 
    hostile(Z).
```

---

### 5.2 Step-by-Step Backward Chaining Proof Trace

When evaluating query `?- criminal(west).`:

```
   [CALL]  criminal(west)
   [CALL]  american(west)
   [EXIT]  american(west)            --> TRUE (matches fact)
   [CALL]  weapon(Y)
   [CALL]  missile(Y)
   [EXIT]  missile(m1)               --> TRUE, Y = m1
   [EXIT]  weapon(m1)                --> TRUE, Y = m1
   [CALL]  sells(west, m1, Z)
   [CALL]  missile(m1)
   [EXIT]  missile(m1)               --> TRUE
   [CALL]  owns(nono, m1)
   [EXIT]  owns(nono, m1)            --> TRUE, Z binds to nono
   [EXIT]  sells(west, m1, nono)      --> TRUE
   [CALL]  hostile(nono)
   [CALL]  enemy(nono, america)
   [EXIT]  enemy(nono, america)      --> TRUE
   [EXIT]  hostile(nono)             --> TRUE
   [EXIT]  criminal(west)            --> PROVEN TRUE!
```

---

## 📖 Module 6: Syntax Traps, Universal Recursion & Viva Preparation

### 6.1 The 5 Fatal "No Cheat Sheet" Traps

| # | Trap Concept | Correct Syntax | Fatal Mistake | Why it Breaks |
| :--- | :--- | :--- | :--- | :--- |
| **1** | **Atoms vs Variables** | Atoms: `john`, `paris`<br>Vars: `X`, `Person` | `male(John).`<br>`father_of(x, y).` | Capitalizing turns constants into variables! Lowercasing variables makes them fixed atoms! |
| **2** | **Arithmetic Evaluation** | `Total is T1 + T2.` | `Total = T1 + T2.` | `=` is syntactic pattern matching (unification); it produces the literal expression `1 + 2`, NOT `3`. `is` forces arithmetic calculation. |
| **3** | **Less Than or Equal** | `Price =< Budget.` | `Price <= Budget.` | Prolog syntax only recognizes `=<`. Writing `<=` throws a fatal syntax error. |
| **4** | **Inequality Operator** | `X \= Y.` | `X != Y.` or `X <> Y.` | Prolog uses `\=` for term disequality. Essential in siblings: `sister_of(X, Y) :- ..., X \= Y.` |
| **5** | **Statement Terminator** | Ending with `.` (period) | Forgetting the `.` | Prolog will wait indefinitely for input or throw a syntax error. |

---

### 6.2 The Universal Recursion Template

Every transitive relationship (ancestors, routes, prerequisites, reachability) follows this exact 2-clause pattern:

```prolog
% CLAUSE 1 (Base Case): Direct 1-step relationship
reach(From, To) :- 
    step(From, To).

% CLAUSE 2 (Recursive Case): Step to intermediate, then recurse
reach(From, To) :- 
    step(From, Intermediate), 
    reach(Intermediate, To).
```

#### ⚠️ The Fatal Left-Recursion Trap:
If you write the recursive call first:
```prolog
% WRONG! CAUSES INFINITE RECURSION (Stack Overflow crash)
reach(From, To) :- 
    reach(From, Intermediate),  % Never do this!
    step(Intermediate, To).
```
Because Prolog searches **Depth-First left-to-right**, it will call `reach` over and over with unbound variables before ever inspecting the base facts, resulting in `ERROR: Out of local stack`.

---

### 6.3 4-Port Debugger Model

When you run `?- trace.`, SWI-Prolog steps through execution using Byrd's 4-port box model:

```
               +---------------------------+
  [CALL] ----> |                           | ----> [EXIT]  (Goal Succeeded)
               |           GOAL            |
  [FAIL] <---- |                           | <---- [REDO]  (Backtrack for alternative)
               +---------------------------+
```

1. **CALL:** The engine starts trying to prove the goal.
2. **EXIT:** The goal was successfully proved, and variables are bound.
3. **REDO:** The user entered `;` or a downstream goal failed, causing Prolog to backtrack and search for an alternate solution.
4. **FAIL:** No clause or fact matches the goal; the branch is abandoned.

---

### 6.4 Viva & Theory Oral Exam Master Q&A

#### Q1: What programming paradigm does Prolog belong to?
**Answer:** **Declarative programming**. The programmer specifies *what* the problem and relations are using facts and rules, rather than *how* to calculate the answer with procedural control flow.

#### Q2: What is a Horn Clause?
**Answer:** A Horn Clause is a clause with at most **one positive (head) literal**. In Prolog:
- **Definite clause / Rule:** `H :- B1, B2.` (Head is positive, body literals are negative in clausal form: $H \lor \neg B1 \lor \neg B2$).
- **Fact:** `H.` (Single positive literal with empty body).
- **Goal / Query:** `?- B1, B2.` (No positive literal).

#### Q3: How does Prolog search for answers?
**Answer:** It uses **Depth-First Search (DFS)** on the SLD tree with **automatic backtracking**, evaluating clauses from top-to-bottom and subgoals from left-to-right.

#### Q4: What is Unification?
**Answer:** Unification is the algorithm that determines if two logical terms can be made identical by binding their variables. For example, `parent(X, bob)` unifies with `parent(tom, bob)` by binding `X = tom`.

#### Q5: What is "Negation as Failure" (NAF)?
**Answer:** Prolog uses the **Closed-World Assumption (CWA)**. If a goal cannot be proved true from the known facts and rules in the database, Prolog assumes it is false (`\+ Goal`).
