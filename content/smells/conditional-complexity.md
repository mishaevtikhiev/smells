---
slug: "conditional-complexity"
meta:
  last_update_date: 2022-02-26
  title: "Conditional Complexity"
  cover: "/logos/logo-text-2560x1280.png"
  known_as:
    - Repeated Switching
    - Switch Statement
    - Conditional Complexity
    - Prefer Polymorphism to if/else or switch/case
categories:
  expanse: "Within"
  obstruction:
    - Object Oriented Abusers
  occurrence:
    - Conditional Logic
  tags:
    - ---
  smell_hierarchies:
    - Code Smell
    - Design Smell
    - Implementation Smell
relations:
  related_smells:
    - name: Callback Hell
      slug: callback-hell
      type:
        - family
    - name: Combinatorial Explosion
      slug: combinatorial-explosion
      type:
        - family
    - name: Flag Arguments
      slug: flag-argument
      type:
        - caused
    - name: Loops
      slug: loops
      type:
        - caused
    - name: Null Check
      slug: null-check
      type:
        - caused
    - name: Long Parameter List
      slug: long-parameter-list
      type:
        - caused
problems:
  general:
    - Comprehensibility
    - More Complex API
    - Unnecessary Indirection
    - Increased Test Complexity
  violation:
    principles:
      - Open-Closed
    patterns:
      - ---
refactors:
  - Use a Guard Clause
  - Extract Conditional
  - Replace with Polymorphism
  - Use Strategy Pattern
  - Use Null Object
  - Use Functional Programming Based Solution
history:
  - author: "Steve Smith"
    type: "origin"
    named_as:
      - Conditional Complexity
    regarded_as:
      - Code Smell
    source:
      year: 2013
      authors:
        - Steve Smith
      name: "Refactoring Fundamentals"
      type: "course"
      href:
        direct_url: "https://www.pluralsight.com/courses/refactoring-fundamentals"
  - author: "Martin Fowler"
    type: "update"
    named_as:
      - Repeated Switching
    regarded_as:
      - Code Smell
    source:
      year: 2018
      authors:
        - Martin Fowler
      name: "Refactoring: Improving the Design of Existing Code (3rd Edition)"
      type: "book"
      href:
        isbn_13: "978-0134757681"
  - author: "Martin Fowler"
    type: "parentage"
    named_as:
      - Switch Statement
    regarded_as:
      - Code Smell
    source:
      year: 1999
      authors:
        - Martin Fowler
        - Kent Beck (contributor)
        - Don Roberts (contributor)
      name: "Refactoring: Improving the Design of Existing Code"
      type: "book"
      href:
        isbn_13: "978-0201485677"
        isbn_10: "0201485672"
---

## Conditional Complexity

In data-oriented programming, the usage of `switch`-like statements (long, cascading `if` statements, `switch`/`case`, etc.) should be relatively rare. One such switch usually executes code that is scattered around the code base and usually should be replaced with a polymorphism solution.

This smell was originally phrased _Switch Statement_ by Fowler and Beck, back in 1999 [[1](#sources)]. One year later Mantyla noted that such a name is misleading as switch statements are not necessarily implying a code smell but rather just in the situation when they are used instead of viable polymorphism solution. [[2](#sources)] Fowler, 14 years later, in his newest book in 2018 changed the name suggesting _Repeated Switching_, agreeing that fortuitously due to the way he initially phrased it, the conditional statements got too much bad reputation, while he was never unconditionally opposed to existence of all `if`-s and `switch`-es. [[3](#sources)] In the "Clean Code" by Robert Martin, the smell was called "Prefer Polymorphism to if/else or switch/case" which is lengthy, but it hits the nail on the head. [[4](#sources)]

I’m providing a fairly common example of such issue on a `Exporter` class with different file formats available, where for each new feature new `elif` has been added instead of using an Object Oriented solution like the Factory Method.

It is important to keep a class focused on a single concern to make it more robust. Martin Fowler defines responsibility as a reason to change, concluding that “A class should have only one reason to change.” [[3](#sources)]. An example that violates it would be a class which prints a table that handles both the contents of the cells as well as the styling of the table.

Another situation, which is not explicitly mentioned in the sources, would be nested `Try` and `Except`/`Catch` "checklist", where numerous error catching blocks are used instead of one generalized for the situation at hand.

### Causation

The most common way such smell can be created is when instead of creating new class, a conditional switch behavior is used. The first time it happens is not yet a "scenty" smell, but as soon as this is done second time, then this immediately a saturated red flag. [[5](#sources)] Logic blocks grew over time larger and larger and no one bothered to implement an Object Oriented Programming style alternative like decorator, strategy or state. It was easier to add another `else if`.

### Problems

#### **Open-Closed Principle Violation**

Class should be open for extension but closed for modification

#### **More Complex API’s**

#### **Comprehensibility**

If statement blocks make things harder to understand as they require to think about all the possible paths the logic can go.

#### **Unnecessary Indirection**

Most likely, there is a way to execute one method on a polymorphic class instead of switch case'ing.

#### **Increased Test Complexity**

Each subsequent if branch needs each subsequent test.

### Examples

<div class="example-block">

#### Smelly

```py
class Exporter:
    def export(self, export_format: str):
        if export_format == 'wav':
            self.exportInWav()
        elif export_format == 'flac':
            self.exportInFlac()
        elif export_format == 'mp3':
            self.exportInMp3()
        elif export_format == 'ogg':
            self.exportInOgg()
```

#### Solution

```py
class Exporter:
    def export(self, export_format: str):
        exporter = self.get_format_factory(export_format)
        exporter.export()
    def get_format_factory(self, export_format: str):
        if export_format in self.export_format_factories:
            return render_factory[export_format]
        raise MissingFormatException
            ...

```

</div>

### Refactoring:

- Use a Guard Clause
- Extract Conditional
- Replace with Polymorphism
- Use Strategy Pattern
- Use Null Object
- Use Functional Programming Based Solution

---

##### Sources

- [[1](#sources)], [Parentage] - Martin Fowler, _"Refactoring: Improving the Design of Existing Code"_ (1999)
- [[2](#sources)] - Mika Mäntylä, _"Bad Smells in Software - a Taxonomy and an Empirical Study"_ (2003)
- [[3](#sources)] - Marin Fowler, _"Refactoring: Improving the Design of Existing Code (3rd Edition)"_ (2018)
- [[4](#sources)] - Robert Martin, _"Clean Code: A Handbook of Agile Software Craftsmanship"_ (2008)
- [[5](#sources)] - William C. Wake, _"Refactoring Workbook"_ (2004)
- [Origin] - Steve Smith, _"Refactoring Fundamentals"_ (2013)