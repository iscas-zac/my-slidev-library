---
theme: default
# random image from a curated Unsplash collection by Anthony
# like them? see https://unsplash.com/collections/94734566/slidev
background: https://source.unsplash.com/collection/94734566/1920x1080
class: 'text-center'
highlighter: shiki
lineNumbers: false
info: |
  ## Adding new clause method for local search

drawings:
  persist: false
css: unocss
---

# Adding new clause (ANC) method for local search

AAAI-96 Proceedings, CHA and IWAMA


---

# Basic Concepts

especially exotic ones

## cell

A specific assignment of true (or 1) and false (or 0) to all the variables

## covering

a clause A covers a cell C if the truth assignment denoted by C makes A false

## OL( C )

The number of overlaps of a cell C is the number of the clauses that cover C

---

# Unified local search

a framework

## Algorithm

*C* := randomly selected cell

**until** *C* becomes a solution **do**

&emsp;&emsp;**if** *C* is not a local minima

&emsp;&emsp;**then**

&emsp;&emsp;&emsp;&emsp;**do** α

&emsp;&emsp;**else**

&emsp;&emsp;&emsp;&emsp;**do** β

---

# WEIGHT method

weighted local search

## basic idea

$$ OL(C) = \sum_{A\ covers\ C} w(A) $$

## standard approach

In the standard weighted local search, WEIGHT, α = Move-downward and β = Weighting.

---

# ANC method

adding new clauses in local search

## neighboring clause

there exists exactly one variable which appears affirmatively in one and negatively in the other

## approach

α = Move-downward

β = **for** each clause *A* covering *C*

&emsp;&emsp;**do** *B* := a neighbor of *A*

&emsp;&emsp;*X* := resolution *A* and *B*

&emsp;&emsp;add *X* as a new clause

---

# Effect

<img src="/pics/result_fig-2.png" width=400 style="float: right;">

<img src="/pics/result_fig-1.png" width=400>

---

# Analysis

why ANC is better than WEIGHT

<img src="/pics/wander.png" width=200 style="float: right;">

assumptions include:

## WEIGHT revisits same cells

During 69 steps, only 19 different cells are visited.

feature: easily fall back

<img src="/pics/illustration.png" width=200 style="float: right;">

## ANC has smaller clause sizes

WEIGHT's size is roughly 35, while ANC's is roughly 3
