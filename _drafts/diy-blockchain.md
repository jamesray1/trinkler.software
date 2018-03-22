---
layout: post
title: "DIY Blockchain"
author: "R. Trinkler"
---

## Introduction

A _blockchain_ is a [transactional singleton machine with shared-state](https://ethereum.github.io/yellowpaper/paper.pdf). That is a virtual computer with a shared data storage as well as an emulated and shared CPU called a virtual machine. The shared data storage as well as the shared virtual machine are the same on each particiant running this virtual computer - they are canonical.

A _domainchain_ is a domain specific blockchain. An example for a domainchain is the [Asset Management Computer](https://melonport.com).

A _webchain_ is a blockchain that is executable in the browser. An example of a webchain is [Nimiq](https://nimiq.com)

## State

_**Traditional (Data)**: A technology consisting of computer components and recording media that are used to retain digital data. **Modern (State)** Shared data stored in a mapping._

The storage or **State** has two main goals
- lower disk storage footprint
- allow for efficient/light state transition proofs (performance).

## Validity

_**Traditional (CPU)**: The central processing unit (CPU) of a computer is what manipulates data by performing computations. **Modern (VM)** In our case the CPU is virtual (software rather than hardware) and emulated using the hardware CPU. This is called a Virtual Machine (VM)._

The **Virtual Machine** is the execution environment that can modify the Melon chain state. Modifications happen according to the Melon state transistion function.

## Canonicality

_**Modern (One truth):** A new concept, native to blockchain technology. Establishes one truth among many possiblities._
