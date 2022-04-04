---
slug: "fallacious-method-name"
meta:
  last_update_date: 2022-02-22
  title: "Fallacious Method Name"
  cover: "/logos/logo-text-2560x1280.png"
  known_as:
    - '"Set" Method returns'
    - '"Get" Method does not Return'
    - '"Get" Method - More than an Accessor'
    - '"Is" Method - More than a Boolean'
    - Expecting but not getting a collection
    - Expecting but not getting a single instance
    - Not answered question
    - Validation method does not confirm
    - Use Standard Nomenclature Where Possible
categories:
  expanse: "Within"
  obstruction:
    - Lexical Abusers
  occurrence:
    - Names
  tags:
    - ---
  smell_hierarchies:
    - Code Smell
    - Implementation Smell
    - Linguistic Antipattern
relations:
  related_smells:
    - name: "Fallacious Comment"
      slug: fallacious-comment
      type:
        - family
    - name: "Obscured Intent"
      slug: obscured-intent
      type:
        - family
problems:
  general:
    - Decreased Readability
    - Reduced Reliability
  violation:
    principles:
      - ---
    patterns:
      - ---
refactors:
  - Rename Method
history:
  - author: "Marcel Jerzyk"
    type: "origin"
    named_as:
      - Fallacious Method Name
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
  - author: "Venera Arnaoudova"
    type: "parentage"
    named_as:
      - '"Set" Method returns'
      - '"Get" Method does not Return'
      - '"Get" Method - More than an Accessor'
      - '"Is" Method - More than a Boolean'
      - Expecting but not getting a collection
      - Expecting but not getting a single instance
      - Not answered question
      - Validation method does not confirm
    regarded_as:
      - Linguistic Antipattern
    source:
      year: 2015
      authors:
        - Venera Arnaoudova
        - Massimiliano Di Penta
        - Giuliano Antoniol
      name: "Linguistic Antipatterns: What they are and how developers perceive them"
      type: "paper"
      href:
        journal: "Empirical Software Engineering"
        pages: "104--158"
        publisher: "Springer"
        year: 2016
        volume: 21
        number: 1
  - author: "Robert C. Martin"
    named_as:
      - Use Standard Nomenclature Where Possible
    regarded_as:
      - "Code Smell (Name)"
    source:
      year: 2008
      authors:
        - Robert C. Martin
      name: "Clean Code: A Handbook of Agile Software Craftsmanship"
      type: "book"
      href:
        isbn_13: "978-0132350884"
        isbn_10: "9780132350884"
---

## Fallacious Method Name

When I started to think about Code Smells from the comprehensibility perspective (of its lack of) as one of the key factors, I was quite intrigued that it was not researched yet thoroughly, or at least not when researching with focus on _"Code Smell"_ as a keyword. There is the grounded idea about code that is obfuscated from the point of [Obscured Intentions](./obscured-intent.md) or no explanation at all ([Magic Number](./magic-number.md), [Uncommunicative Name](./uncommunicative-name.md)). I felt like there was a missing hole for code which is intentionally being too clever ([Clever Code](./clever-code.md)) or code that is contradicting itself. Luckily, I have found that there is an amazing paper backing my thoughts that addresses some of the things I had in my mind under the name of _Linguistic Antipatterns_ [[1](#sources)]. I have included their subset of the antipatterns under one code smell because listing them out one-by-one would be too granular. The idea behind them can be summarized by one name - _Fallacious Method Name_.

This smell is caused by creating conflicting methods or functions as to their functionality and naming. Over the years, programmers have developed connections between certain words and functionality that should be tied with them together. Going against logical expectations by, for example, creating a `getSomething` function that does not return is confusing and wrong.

### Causation

When we create methods, their name may not be entirely representative of what it will be doing after we finish working on them. Except that one should then go back and correct its name accordingly, to what has been done.

### Problems

#### **Decreased Readability**

Developers need to inspect the function or methods in order to find out they do.

#### **Reduced Reliability**

If someone would like to use a method with a given name, that someone should also get the effect that the name implies.

### Example

<div class="example-block">

#### Smelly

```py
def getFoos() -> Foo:
    ...
    return Foo

def isGoo() -> str:
    ...
    return 'yes'

def setValue(self, value) -> Any:
    ...
    return new_value
```

#### Solution

```py
def getFoos() -> list[Foo]:
    ...
    foos: list[Foo] = ...
    return list[Foo]

def isGoo() -> bool:
    ...
    return True

def setValue(self, value) -> None:
    ...
```

</div>

### Refactoring:

- Rename Method (to remove contradictions)

---

##### Sources

- [Origin] - Marcel Jerzyk, _"Code Smells: A Comprehensive Online Catalog and Taxonomy"_ (2022)
- [Parentage1] - Venera Arnaoudova, Massimiliano Di Penta, Giuliano Antoniol _"Linguistic Antipatterns: What they are and how developers perceive them"_ (2015)
- [Parentage2] - Robert Martin, _"Clean Code: A Handbook of Agile Software Craftsmanship"_ (2008)