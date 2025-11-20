---
layout: project
title: Contextual Control Suite
description: 
img: assets/img/cheetah.png
abstract_subtitle: In Proceedings of the AAAI Conference on Artificial Intelligence (AAAI) 2023.
importance: 3
category: fun
date: 2023-01-03
github: https://github.com/SAIC-MONTREAL/contextual-control-suite
website: https://sites.google.com/view/hyperzero-rl
keywords: simulation, reinforcement learning, control suite
---

In this project, we build the Contextual Control Suite, a modular suite of continuous control environments built on the DeepMind Control Suite, designed for studying zero-shot and transfer generalization in reinforcement learning. In these environments, both reward functions (e.g., desired speed) and dynamics (e.g., body size, weight, inertia) are parametrically varied, enabling systematic evaluation across families of related tasks. This setup provides a controlled suite for contextual MDPs, where agents must generalize behaviours to unseen combinations of reward and dynamics parameters. The Contextual Control Suite was introduced and used to evaluate HyperZero, demonstrating significant improvements over multitask and meta-RL baselines in zero-shot transfer.