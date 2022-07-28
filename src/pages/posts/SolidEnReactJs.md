---
layout: "../../layouts/PostLayout.astro"
setup: |
  import Image from '../../components/Image.astro'
  import Title from '../../components/Title.astro'
title: "SOLID en React (1/5)"
author: "Lucas Angelino"
date: "1 de Julio de 2022"
readtime: "10"
thumnail: "/img/solid_en_react_1.webp"
description: "En este primer articulo de la saga vas a descubrir los conceptos de SOLID en React"
---

<Image src="/img/solid_en_react_1.webp" alt="Img" />

# Motivación

As the software industry grows and makes mistakes, the best practices and good software design principles emerge and conceptualize to avoid repeating the same mistakes in the future. The world of object-oriented programming (OOP) in particular is a goldmine of such best practices, and SOLID is unquestionably one of the more influential ones.

SOLID is an acronym, where each letter represents one out of five design principles which are:

- Single responsibility principle (SRP)
- Open-closed principle (OCP)
- Liskov substitution principle (LSP)
- Interface segregation principle (ISP)
- Dependency inversion principle (DIP)

In this article, we’ll talk about the importance of each principle and see how we can apply the learnings from SOLID in React applications.

Before we begin though, there’s a big caveat. SOLID principles were conceived and outlined with object-oriented programming language in mind. These principles and their explanation heavily rely on concepts of classes and interfaces, while JS doesn’t really have either. What we often think of as “classes” in JS are merely class look-alikes simulated using its prototype system, and interfaces aren’t part of the language at all (although the addition of TypeScript does help a bit). Even more, the way we write modern React code is far from being object-oriented -  if anything, it feels more functional.

The good news though, software design principles such as SOLID are language agnostic and have a high level of abstraction, meaning that if we squint hard enough and take some liberties with interpretation, we’ll be able to apply them to our more functional React code.

So let’s take some liberties

## Single responsibility principle (SRP)

The original definition states that “every class should have only one responsibility”, a.k.a. do exactly one thing. This principle is the easiest to interpret, as we can simply extrapolate the definition to “every function/module/component should do exactly one thing”.

<Image src="/img/solid_in_react_srp.svg" alt="Img" />

Although this component is relatively short now, it is already doing quite a few things - it fetches data, filters it, renders the component itself as well as individual list items. Let’s see how we can break it down.

First of all, whenever we have connected useState and useEffect hooks, it’s a good opportunity to extract them into a custom hook:

<Image src="/img/solid_en_react_2.svg" alt="Img" />

Now our useUsers hook is concerned with one thing only - fetching users from API. It also made our main component more readable, not only because it got shorter, but also because we replaced the structural hooks that you needed to decipher the purpose of with a domain hook the purpose of which is immediately obvious from its name.

Next, let’s look at the JSX that our component renders. Whenever we have a loop mapping over an array of objects, we should pay attention to the complexity of JSX it produces for individual array items. If it’s a one-liner that doesn’t have any event handlers attached to it, it’s totally fine to keep it inline, but for a more complex markup it could be a good idea to extract it into a separate component:

<Image src="/img/solid_en_react_3.svg" alt="Img" />

Just as with a previous change, we made our main component smaller and more readable by extracting the logic for rendering user items into a separate component.

Finally, we have the logic for filtering out inactive users from the list of all users we get from an API. This logic is relatively isolated and it could be reused in other parts of the application, so we can easily extract it into a utility function:

<Image src="/img/solid_en_react_4.svg" alt="Img" />

At this point, our main component is short and straightforward enough that we can stop breaking it down and call it a day. However, if we look a bit closer, we’ll notice that it’s still doing more than it should. Currently, our component is fetching data and then applying filtering to it, but ideally, we’d just want to get the data and render it, without any additional manipulation. So as the last improvement, we can encapsulate this logic into a new custom hook:

<Image src="/img/solid_en_react_5.svg" alt="Img" />

Here we created useActiveUsers hook to take care of fetching and filtering logic (we also memoized filtered data for good measures), while our main component is left to do the bare minimum - render the data it gets from the hook.

Now depending on our interpretation of “one thing”, we can argue that the component is still first getting the data, and then rendering it, which is not “one thing”. We could split it even further, calling a hook in one component and then passing the result to another one as props, but I found very few cases where this is actually beneficial in real-world applications, so let’s be forgiving with the definition and accept “rendering data the component gets” as “one thing”.

To summarize, following the single-responsibility principle, we effectively take a large monolithic piece of code and make it more modular. Modularity is great because it makes our code easier to reason about, smaller modules are easier to test and modify, we’re less likely to introduce unintentional code duplication, and as a result, our code becomes more maintainable.

It should be said, that what we’ve seen here is a contrived example, and in your own components you may find that the dependencies between different moving parts are much more intertwined. In many cases, this could be an indication of poor design choices - using bad abstractions, creating universal do-it-all components, incorrectly scoping the data, etc., and thus can be untangled with a broader refactoring.
