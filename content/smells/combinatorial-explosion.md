---
slug: "combinatorial-explosion"
meta:
  last_update_date: 2022-02-20
  title: "Combinatorial Explosion"
  cover: "/logos/logo-text-2560x1280.png"
  known_as:
    - ---
categories:
  expanse: "Within"
  obstruction:
    - Bloaters
  occurrence:
    - Responsibility
  tags:
    - ---
  smell_hierarchies:
    - Code Smell
    - Design Smell
relations:
  related_smells:
    - name: Conditional Complexity
      slug: combinatorial-explosion
      type:
        - family
    - name: Parallel Inheritance Hierarchies
      slug: parallel-inheritance-hierarchies
      type:
        - family
problems:
  general:
    - ---
  violation:
    principles:
      - Don't Repeat Yourself
      - Open Closed
    patterns:
      - Decorator
      - Strategy
      - State
refactors:
  - Replace Inheritance with Delegation
  - Tease Apart Inheritance
history:
  - author: "William C. Wake"
    type: "origin"
    named_as:
      - Combinatorial Explosion
    regarded_as:
      - Code Smell
    source:
      year: 2004
      authors:
        - William C. Wake
      name: "Refactoring Workbook"
      type: "book"
      href:
        isbn_13: "978-0321109293"
        isbn_10: "0321109295"
---

## Combinatorial Explosion

The Combinatorial Explosion happens when there is a lot of code that does almost the same, but almost. The number of cases needed to cover every possible path is massive and so is the number of methods. You can grasp a solid intuition of this smell by thinking about code blocks that differ between each other only by the quantities of data or objects used in them. Wake specifies that "(...) this is a relative of [Parallel Inheritance Hierarchies](./parallel-inheritance-hierarchies.md) Code Smell, but everything has been folded into one hierarchy." [[1](#sources)].

### Causation

What should be independent decisions instead get implemented via a hierarchy. Lets suppose that someone organised the code that query an API by a specific method with specific set-in conditions and data. Sooner or later, there is just so much of these methods just as the need for different queries rises in demand. This is bad by the initial design solution.

### Problems:

#### **Don't Repeat Yourself Principle Violation**

Introducing new functionality requires multiple versions to be introduced in various places.

#### **Open-Closed Principle Violation**

Class is not closed for modification, if it "prompts" the developer to add another `elif`.

### Example

<div class="example-block">

#### Smelly

```py
class Minion:
    name: str
    state: 'ready'

    def action(self):
        if self.state == 'ready':
            self.animate('standing')
        elif self.state == 'fighting':
            self.animate('fighting')
        elif self.state == 'resting':
            self.animate('resting')

    def next_state(self):
        if self.state == 'ready':
            return 'fighting'
        elif self.state == 'fighting':
            return 'resting'
        elif self.state == 'resting':
            return 'ready'

    def animate(self, animation: str):
        print(f"{self.name} is {animation}!")

```

#### Solution

```py
class State(ABC):
    @abstractmethod
    def next() -> State:
        """ Return next State """

    @abstractmethod
    def animate() -> str:
        """ Returns a text-based animation """

class Ready(State):
    def next():
        return States.FIGHT

    def animate():
        return 'standing'

class Fight(State):
    def next():
        return States.REST

    def animate():
        return 'fighting'

class Rest(State):
    def next():
        return States.READY

    def animate():
        return 'resting'

class States(Enum):
    READY: State = Ready
    FIGHT: State = Fight
    REST: State = Rest

class Minion:
    name: str
    state: State = States.Ready

    def action(self):
        self.state.animate()

    def next_state(self):
        self.state = self.state.next()

    def animate(self, animation: str):
        print(f"{self.name} is {self.state.animate()}!")
```

</div>

### Refactoring

- Replace Inheritance with Delegation
- Tease Apart Inheritance
- It's quite hard fix, as the existence of this Code Smell (Design Smell) occurs as soon as the design of the system is decided.

##### Sources

- [[1](#sources)], [Origin] - William C. Wake, _"Refactoring Workbook"_ (2004)