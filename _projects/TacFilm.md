---
layout: project
title: Tactile Modality Fusion for Vision-Language-Action (VLA) Models
description:
img: assets/img/tactile-vla.png
hide_hero: true
hide_date: true
authors: >
  <a href="https://charliem7.github.io/" target="_blank"><strong>Charlotte Morissette</strong></a><sup>1,2</sup>,
  <a href="https://aminabyaneh.github.io/" target="_blank">Amin Abyaneh</a><sup>1,2</sup>,
  <a href="https://weidi-chang.github.io/" target="_blank">Wei-Di Chang</a><sup>3</sup>,
  <a href="https://anashoussaini.github.io/" target="_blank">Anas Houssaini</a><sup>1,2</sup>,
  <a href="https://cim.mcgill.ca/~dmeger/" target="_blank">David Meger</a><sup>1,2</sup>,
  <a href="https://sites.google.com/site/hsiuchinlin/" target="_blank">Hsiu-Chin Lin</a><sup>3</sup>,
  <a href="https://research.nvidia.com/person/jonathan-tremblay" target="_blank">Jonathan Tremblay</a><sup>3</sup>,
  <a href="https://cim.mcgill.ca/~dudek/" target="_blank">Gregory Dudek</a><sup>1,2</sup>
affiliations: >
  <sup>1</sup>&nbsp;McGill University &nbsp;&nbsp;&middot;&nbsp;&nbsp; <sup>2</sup>&nbsp;Mila &mdash; Quebec AI Institute &nbsp;&nbsp;&middot;&nbsp;&nbsp; <sup>3</sup>&nbsp;NVIDIA
importance: 1
category: work
date: 2026-01-15
paper: https://arxiv.org/abs/2603.14604
code_soon: true
data_soon: true
venues:
  - name: "Accepted to ECCV 2026"
    url: https://eccv.ecva.net/
  # - name: "Accepted to CVPR ActiVis Workshop"
  #   url: https://activis-workshop.github.io/
keywords: Vision-Language-Action (VLA) Models, Tactile Sensing, Multimodal Fusion, Robot Learning
related_publications: true
---

{% include video.liquid path="assets/video/TacFiLM_Supplementary.mp4" class="img-fluid rounded" controls=true muted=true loop=true autoplay=true caption="TacFiLM in action on contact-rich manipulation." %}


<h2 class="text-center">Summary</h2>

Vision-Language-Action (VLA) models are a powerful recipe for general-purpose robots, but a camera cannot *feel*, and that is exactly what precise, contact-rich tasks demand. **TacFiLM** brings touch to a pretrained VLA through feature-wise linear modulation (FiLM): instead of bolting on extra tactile tokens, it lets tactile signals directly condition the model's visual features. The result keeps the vision-language backbone intact, adds almost no inference overhead, and adapts from modest data, while improving success and force stability on hard insertion tasks. This work was done in collaboration with NVIDIA Research.

## The problem with vision-only manipulation

VLA models map an image and a language instruction straight to actions, and they generalize impressively thanks to internet-scale pretraining. But whether a peg is fully seated, how much a surface grips, or whether the gripper is touching at all simply does not show up reliably in an RGB frame, and occlusion hides the contact at the very moment it matters. Visuo-tactile sensors like [DIGIT](https://digit.ml/) close that gap with a dense image of the contact surface; the real question is how to fold that signal into a VLA without paying for it in compute, data, or stability.

{% include figure.liquid loading="lazy" path="assets/img/tacfilm_overview.png" class="img-fluid rounded" zoomable=true caption="Vision tells the policy where things are; touch tells it what is actually happening at the contact. TacFiLM adds the tactile channel to a pretrained VLA so the model can reason about contact it cannot see." %}

## Why naive fusion is expensive

The obvious approach is to encode the tactile image into extra tokens and concatenate them to the input. It works, but it is costly where it hurts: more tokens mean longer sequences and slower inference, the reshuffled attention forces the model to relearn how to use its own visual features (demanding scarce paired tactile data), and heavy retraining erodes the very pretraining that made the VLA useful. We wanted fusion that is *additive*, not *invasive*: light on overhead, gentle on the backbone, and learnable from a handful of demonstrations.

## TacFiLM: conditioning vision on touch

TacFiLM integrates touch through [feature-wise linear modulation (FiLM)](https://arxiv.org/abs/1709.07871). A tactile encoder reads the contact image and predicts per-channel **scale** (gamma) and **shift** (beta) values; each visual feature is then scaled and offset accordingly. In effect, touch is allowed to reweight and bias what vision sees, amplifying what matters at contact and damping the rest, all without lengthening the sequence the transformer processes. Because we condition rather than re-architect, the [OpenVLA](https://openvla.github.io/) backbone stays intact, latency barely moves, and adaptation is fast and data-efficient. We also reuse **pretrained tactile encoders** such as [T3](https://arxiv.org/abs/2406.13640) and [Sparsh](https://ai.meta.com/research/publications/sparsh-self-supervised-touch-representations-for-vision-based-tactile-sensing/), so TacFiLM benefits from tactile pretraining the same way the VLA already benefits from vision-language pretraining.

{% include figure.liquid loading="lazy" path="assets/img/tacfilm_method.png" class="img-fluid rounded" zoomable=true caption="The TacFiLM architecture. A pretrained tactile encoder maps the contact image to FiLM parameters that modulate the visual features inside the VLA backbone, leaving the vision-language pathway and the action decoder otherwise untouched." %}

## Results

We test TacFiLM on contact-rich **insertion** with a [Franka Panda](https://franka.de/) arm and a [DIGIT](https://digit.ml/) sensor, the regime where vision-only policies struggle most. Conditioning on touch lifts both **success rate** and **force stability**, with the largest gains on **tight-tolerance** insertions where feedback carries the most information, and it does so without the cost of token-level fusion. Encouragingly, pretrained tactile encoders transfer cleanly, hinting that touch foundation models can become a reusable building block for VLAs.

{% include figure.liquid loading="lazy" path="assets/img/tacfilm_exps.png" class="img-fluid rounded" zoomable=true caption="Real-robot insertion experiments with a Franka Panda and a DIGIT tactile sensor. TacFiLM improves success rates and force stability over vision-only baselines, with the biggest improvements on tight-tolerance insertions." %}

*Code and data will be released soon, watch this page. More media and qualitative rollouts will be added here as they become available.*

## Citation

```bibtex
@article{morissette2026tactile,
  title   = {Tactile Modality Fusion for Vision-Language-Action Models},
  author  = {Morissette, Charlotte and Abyaneh, Amin and Chang, Wei-Di and
             Houssaini, Anas and Meger, David and Lin, Hsiu-Chin and
             Tremblay, Jonathan and Dudek, Gregory},
  journal = {arXiv preprint arXiv:2603.14604},
  year    = {2026}
}
```
