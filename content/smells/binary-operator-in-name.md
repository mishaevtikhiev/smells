---
slug: "binary-operator-in-name"
meta:
  last_update_date: 2022-02-16
  title: "Binary Operator in Name"
  cover: "/logos/logo-text-2560x1280.png"
  known_as:
    - ---
categories:
  expanse: "Within"
  obstruction:
    - Couplers
  occurrence:
    - Names
  tags:
    - ---
  smell_hierarchies:
    - Code Smell
    - Linguistic Smell
relations:
  related_smells:
    - name: Flag Arguments
      slug: flag-argument
      type:
        - causes
    - name: Status Variable
      slug: status-variable
      type:
        - co-exist
    - name: Side Effects
      slug: side-effects
      type:
        - antagonistic
problems:
  general:
    - ---
  violation:
    principles:
      - Single Responsibility
    patterns:
      - ---
refactors:
  - Extract Method
history:
  - author: "Marcel Jerzyk"
    type: "origin"
    named_as:
      - Binary Operator in Name
    regarded_as:
      - Code Smell
    source:
      year: 2022
      authors:
        - Marcel Jerzyk
      name: "Code Smells: A Comprehensive Online Catalog and Taxonomy"
      type: "thesis"
      href:
        direct_url: "Marcel Jerzyk Source TBA"
---

## Binary Operator in Name

This is straightforward - method or function names that have a binary bitwise operators like `AND` and `OR` are obvious candidates of undisguised violators of **Single Responsibility Principle** right out there in the open. If the method name has _`and`_ in its name and then it does two different things, then one might ask why its not split in half to do these two different things separately? Moreover, if the method name has _`or`_ then not only it does two different things, but additionally and most likely it has a stinky [Flag Argument](./flag-argument.md) which is yet another Code Smell.

This might happen not only in the method names, even though it is the place to look for in the vast majority of this kind of smell, but also in variables.

### Problems:

#### Single Responsibility Principle Violation

Names with conjunction words strongly suggested that something is not responsible for just one thing.

### Example

<div class="example-block">

#### Smelly

```py
def render_and_save():
    # render logic
    ...
    # save logic
    ...
```

#### Solution

```py
def render():
    # render logic
    ...

def save():
    # save logic
    ...
```

### Refactoring:

- _Extract Method_

---

##### Sources