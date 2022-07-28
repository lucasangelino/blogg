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
