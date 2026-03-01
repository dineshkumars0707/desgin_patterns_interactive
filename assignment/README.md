# Assignment — Design Patterns in Practice

## Overview

This folder contains **10 coding problems**, each built around a real-world scenario
written in deliberately bad Java code. Your job is to:

1. **Understand** what the code currently does and why it hurts.
2. **Identify** the design pattern that solves the stated problem.
3. **Refactor** the code using that pattern — add as many new classes and files as
   you need inside the problem folder.

> **Work in pairs.** Discuss the problem together before writing any code.  
> Talk through *why* the current design is painful before reaching for a solution.

---

## Ground Rules

| Rule | Detail |
|------|--------|
| 📖 Read first | Always read `Main.java` — the problem statement lives in the top comment block. |
| 🔍 Explore the bad code | Read every `.java` file in the folder before touching anything. |
| 🤝 Pair up | One person drives (types), one person navigates (thinks). Swap roles each problem. |
| 🚫 No peeking | Do not look at the `good_code` folders in the main repo until you have submitted. |
| ✅ Keep `Main.java` compiling | Your refactored code must produce the same observable output as the original. |
| 🗂️ Stay in the folder | All your new files go inside the same `problem_N/` folder. |

---

## The 10 Problems

| # | Folder | Scenario | Core Pain |
|---|--------|----------|-----------|
| 1 | [problem_1](problem_1/) | Document export (PDF / HTML / Markdown) | One class handles every format with `if-else` |
| 2 | [problem_2](problem_2/) | RPG character equipment (Fantasy / Sci-Fi themes) | Three separate `if-else` methods mix themes |
| 3 | [problem_3](problem_3/) | Text editor with undo | Operations called directly — no history, no undo |
| 4 | [problem_4](problem_4/) | Reading list traversal | Public array exposed; callers use raw index loops |
| 5 | [problem_5](problem_5/) | Restaurant menu with sub-menus | Items and sub-menus handled by different methods; no nesting |
| 6 | [problem_6](problem_6/) | Traffic light state machine | Magic string state + `if-else` in every method |
| 7 | [problem_7](problem_7/) | E-commerce discount pricing | All discount algorithms in one growing `if-else` |
| 8 | [problem_8](problem_8/) | Analytics report pipeline | Validate + send steps copy-pasted across every generator |
| 9 | [problem_9](problem_9/) | Web app login flow | Callers manually wire and call four subsystems in order |
| 10 | [problem_10](problem_10/) | Pizza topping customisation | Every combination is a separate class — combinatorial explosion |

---

## Problem Details

### Problem 1 · Document Converter
**Folder:** [problem_1](problem_1/)

A document management system exports content in **PDF, HTML, and Markdown** formats.
A single `DocumentConverter` class handles all three with `if-else` branching.

- Adding a new format (e.g. EPUB) means editing `DocumentConverter`.
- Each format's logic is tangled — impossible to test in isolation.
- The class grows with every new format.

**Your task:** Refactor so each format is its own class, and `DocumentConverter`
never needs to change when a new format is introduced.

---

### Problem 2 · RPG Character Factory
**Folder:** [problem_2](problem_2/)

A role-playing game supports **Fantasy** and **Sci-Fi** themes. Each theme needs a
matched set: Weapon, Armor, and Mount. `CharacterFactory` currently uses a separate
`if-else` method for each item.

- Nothing prevents mixing a Fantasy sword with a Sci-Fi hover bike.
- Adding a new theme (e.g. "Steampunk") requires editing all three methods.
- Theme logic is scattered rather than grouped.

**Your task:** Refactor so each theme's complete equipment set is created together,
and a new theme only requires adding new classes — not editing existing ones.

---

### Problem 3 · Text Editor with Undo
**Folder:** [problem_3](problem_3/)

A text editor supports **type**, **delete**, and **bold** operations.
Users expect Ctrl+Z to undo the last action, but operations are invoked directly
with no record of what was done.

- No history → no undo.
- Implementing undo/redo later would require duplicating inverse logic everywhere.
- Operations cannot be queued, logged, or replayed.

**Your task:** Refactor so every operation is an object with `execute()` and
`undo()` methods, and a history stack enables multi-level undo.

---

### Problem 4 · Reading List Iterator
**Folder:** [problem_4](problem_4/)

A reading list stores books in a **public array**. Callers access `books[i]` and
`list.size` directly.

- Changing the internal structure (array → `LinkedList`) breaks all callers.
- No way to offer alternative traversal orders (alphabetical, reversed, by genre)
  without rewriting every caller.
- The collection's internals are fully exposed — encapsulation is violated.

**Your task:** Make the collection private and expose a standard traversal
interface so callers never touch the internal structure.

---

### Problem 5 · Restaurant Menu
**Folder:** [problem_5](problem_5/)

A restaurant menu has **items** (e.g. "Burger") and **sub-menus**
(e.g. "Drinks" → ["Coke", "Water"]). Items and sub-menus are handled by
completely different code paths.

- Sub-menus cannot be nested inside sub-menus.
- Callers must track what is an item and what is a sub-menu separately.
- Adding a third level requires yet another method and more caller complexity.

**Your task:** Refactor so items and sub-menus share one interface and sub-menus
can contain other sub-menus to any depth — callers treat them identically.

---

### Problem 6 · Traffic Light
**Folder:** [problem_6](problem_6/)

A traffic light cycles **Red → Green → Yellow → Red** and supports an emergency
override to force green. All state logic uses magic strings and `if-else` blocks
scattered across every method.

- A typo like `"redd"` is not caught at compile time.
- Adding a new state (e.g. "flashing yellow") requires editing every method.
- State-specific behaviour is not grouped — it is fragmented.

**Your task:** Refactor so each state is its own class with its own behaviour.
Adding a new state should only require adding a new class.

---

### Problem 7 · Pricing Service
**Folder:** [problem_7](problem_7/)

An e-commerce checkout applies discounts: **no discount, 10% off, 20% off,
flat $5 off, and BOGO**. All logic lives in one method with a growing `if-else`.

- Adding a "loyalty points" discount means editing `PricingService`.
- Algorithms cannot be reused across different product categories.
- Discounts cannot be swapped at runtime (e.g. based on membership tier).

**Your task:** Refactor so each discount algorithm is its own class, and
`PricingService` receives the chosen algorithm as a dependency instead of
picking it with `if-else`.

---

### Problem 8 · Report Generator Pipeline
**Folder:** [problem_8](problem_8/)

An analytics platform generates reports from a **database** and a **REST API**.
Both follow the same four steps: *fetch → validate → format → send by email*.
The validate and send steps are copy-pasted into every generator class.

- A bug fix in "send email" must be applied in every class separately.
- Adding a third source (e.g. CSV) means duplicating the full pipeline again.
- Common steps are not in one authoritative place.

**Your task:** Define the pipeline once in a base class with common steps
implemented there. Subclasses only override the source-specific steps.

---

### Problem 9 · Login Facade
**Folder:** [problem_9](problem_9/)

A web-app login flow coordinates **four subsystems** in order:
`AuthService → ProfileService → RecommendationService → AuditLogger`.
Every caller (controller, CLI, test) must know all four, wire them manually,
and call them in the correct sequence.

- Adding a new step (e.g. 2FA check) means updating every single caller.
- Callers are coupled to four services when they only need "login".
- Coordination logic is duplicated everywhere login is triggered.

**Your task:** Introduce a single class that hides all four subsystems behind
one `login(username, password)` method. Callers depend on that class alone.

---

### Problem 10 · Pizza Toppings
**Folder:** [problem_10](problem_10/)

A pizza shop lets customers customise pizzas with toppings: **Extra Cheese,
Olives, Jalapeños**, etc. Every unique combination is its own class
(`MargheritaWithExtraCheese`, `PepperoniWithOlives`, …).

- 2 bases × 4 toppings already produces dozens of classes.
- Adding one new topping requires a new class for *every* base pizza.
- Toppings cannot be stacked dynamically at runtime.
- `getDescription()` and `getCost()` logic is duplicated everywhere.

**Your task:** Refactor so toppings wrap a base pizza dynamically at runtime.
Each topping adds its own cost and label without creating new subclasses.

---

## Pair-Programming Workflow

Follow this rhythm for **each problem**:

```
1. READ (both)        — read Main.java and all other files silently
2. DISCUSS (both)     — what is broken? what would make it better?
3. IDENTIFY (both)    — agree on the design pattern before writing code
4. SKETCH (navigator) — draw the new class diagram on paper / whiteboard
5. CODE (driver)      — implement the refactoring
6. REVIEW (swap)      — swap roles; reviewer checks correctness and pattern fit
7. VERIFY (both)      — confirm Main.java still compiles and output is the same
```

---

## Evaluation Checklist

Before moving to the next problem, confirm:

- [ ] The original `bad_code` problem is clearly understood and can be explained.
- [ ] The correct design pattern was chosen and can be justified.
- [ ] The new classes follow the pattern's roles (e.g. interface, concrete implementations, context).
- [ ] `Main.java` compiles and produces the same output as before.
- [ ] No new `if-else` on type strings was introduced.
- [ ] The solution is open for extension (adding a new variant requires only a new class).


