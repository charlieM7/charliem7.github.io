---
layout: project
title: Tactile Modality Fusion for Vision-Language-Action (VLA) Models
description:
img: assets/img/tacfilm_overview.png
hide_hero: true
hide_date: true
authors: >
  <a href="https://charliem7.github.io/" target="_blank"><strong>Charlotte Morissette</strong></a><sup>1,2</sup>,
  <a href="https://aminabyaneh.github.io/" target="_blank">Amin Abyaneh</a><sup>1,2</sup>,
  <a href="https://weidi-chang.github.io/" target="_blank">Wei-Di Chang</a><sup>1,2</sup>,
  <a href="https://anashoussaini.github.io/" target="_blank">Anas Houssaini</a><sup>1,2</sup>,
  <a href="https://cim.mcgill.ca/~dmeger/" target="_blank">David Meger</a><sup>1,2</sup>,
  <a href="https://sites.google.com/site/hsiuchinlin/" target="_blank">Hsiu-Chin Lin</a><sup>1,2</sup>,
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
rollout_videos:
  - src: assets/video/circle3mm.mp4
    label: Circle Peg Insertion
  - src: assets/video/square3mm.mp4
    label: Square Peg Insertion
  - src: assets/video/pentagon3mm.mp4
    label: Pentagon Peg Insertion
  - src: assets/video/hdmi.mp4
    label: HDMI Cable Plug
  - src: assets/video/usb.mp4
    label: USB Cable Plug
  - src: assets/video/drawer.mp4
    label: Open Drawer

id_results:
  - "OpenVLA-OFT,58.1,12.4,14.94,126.7"
  - "TactileConcat,64.8,10.5,10.27,113.0"
  - "!!Cross-Attn,48.0,12.0,13.43,149.9"
---

{% include video.liquid path="assets/video/supplementary_final.mp4" class="img-fluid rounded" controls=true muted=true loop=true autoplay=true %}

<h2 class="text-center">Summary</h2>

<div class="row justify-content-center" markdown="1">
<div class="col-md-10"  markdown="1">

<!-- Vision-Language-Action (VLA) models are a powerful recipe for general-purpose robots, but a camera cannot *feel*, and that is exactly what precise, contact-rich tasks demand. **TacFiLM** brings touch to a pretrained VLA through feature-wise linear modulation (FiLM): instead of bolting on extra tactile tokens, it lets tactile signals directly condition the model's visual features. The result keeps the vision-language backbone intact, adds almost no inference overhead, and adapts from modest data, while improving success and force stability on hard insertion tasks. This work was done in collaboration with NVIDIA Research. -->

While advances in vision-language-action models (VLAs) have introduced robot policies that are both generalizable and semantically grounded, these models mainly rely on vision-based perception. Vision alone, however, cannot capture the complex interaction dynamics that occur during contact-rich manipulation, including contact forces, surface friction, compliance, and shear. Recent attempts to integrate tactile signals into VLA models often increase complexity through token concatenation or large-scale pretraining, yet the heavy computational demands of behaviour models necessitate lightweight fusion strategies.
<br>
<br>
We propose **TacFiLM** to address these challenges, a lightweight modality-fusion approach that integrates visual-tactile signals into vision-language-action (VLA) models. Our approach outlines a post-training finetuning approach that conditions intermediate visual features on pretrained tactile representations using featurewise linear modulation (FiLM). 
<br>
<br>
Experimental results on insertion and drawer opening tasks demonstrate consistent improvements in success rate, direct task performance, completion time, and force stability across both in-distribution and out-of-distribution tasks. Together, these results support our method as an effective approach to integrating tactile signals into VLA models, improving contact-rich manipulation behaviours.

</div>
</div>

<div class="tldr-box" markdown="1">
## **TLDR**
**Our main contributions are summarized as follows:**
* TacFiLM, a novel lightweight modality fusion approach that integrates tactile signals
through image conditioning.
* Comprehensive experiments showing that TacFiLM improves success rates
by up to 50% with shorter episodes and reduced contact forces compared to
concatenation and cross-attention-based fusion
* An investigation of the use of pretrained tactile encoders such as [Sparsh](https://ai.meta.com/research/publications/sparsh-self-supervised-touch-representations-for-vision-based-tactile-sensing/)
and [T3](https://arxiv.org/abs/2406.13640) in fusing tactile signals into VLA models.

</div>
<style>
  .tldr-box {
  background: #f5f5f5;
  border-radius: 10px;
  padding: 1.5rem 2rem;
  margin: 1rem 0 1.5rem;
  width: 100vw;
  position: relative;
  left: 50%;
  right: 50%;
  margin-left: -50vw;
  margin-right: -50vw;
}
.tldr-box > * {
  max-width: 900px;
  margin-left: auto;
  margin-right: auto;
}
.tldr-box > p {
  margin-top: 0 !important;
  margin-bottom: 0.5rem !important;
  margin-left: auto !important;
  margin-right: auto !important;
}
.tldr-box > ul {
  margin-top: 0 !important;
  margin-bottom: 0 !important;
  margin-left: auto !important;
  margin-right: auto !important;
  padding-top: 0 !important;
}
</style>

{% include figure.liquid loading="lazy" path="assets/img/tacfilm_overview.png" class="img-fluid rounded" zoomable=true caption="Vision tells the policy where things are; touch tells it what is actually happening at the contact. TacFiLM adds the tactile channel to a pretrained VLA so the model can reason about contact it cannot see." %}

## TacFiLM: conditioning vision on touch

TacFiLM integrates touch through [feature-wise linear modulation (FiLM)](https://arxiv.org/abs/1709.07871). A tactile encoder reads the contact image and predicts per-channel **scale** (gamma) and **shift** (beta) values; each visual feature is then scaled and offset accordingly. In effect, touch is allowed to reweight and bias what vision sees, amplifying what matters at contact and damping the rest, all without lengthening the sequence the transformer processes. Because we condition rather than re-architect, the [OpenVLA-OFT](https://openvla-oft.github.io/) backbone stays intact, latency barely moves, and adaptation is fast and data-efficient. We also reuse **pretrained tactile encoders** such as [T3](https://arxiv.org/abs/2406.13640) and [Sparsh](https://ai.meta.com/research/publications/sparsh-self-supervised-touch-representations-for-vision-based-tactile-sensing/), so TacFiLM benefits from tactile pretraining the same way the VLA already benefits from vision-language pretraining.

{% include figure.liquid loading="lazy" path="assets/img/tacfilm_method.png" class="img-fluid rounded" zoomable=true caption="The TacFiLM architecture. A pretrained tactile encoder maps the contact image to FiLM parameters that modulate the visual features inside the VLA backbone, leaving the vision-language pathway and the action decoder otherwise untouched." %}

## Tasks
{% include video_carousel.liquid videos=page.rollout_videos %}

## Results

We test TacFiLM on contact-rich **insertion** with a [Franka Panda](https://franka.de/) arm and a [DIGIT](https://digit.ml/) sensor, the regime where vision-only policies struggle most. Conditioning on touch lifts both **success rate** and **force stability**, with the largest gains on **tight-tolerance** insertions where feedback carries the most information, and it does so without the cost of token-level fusion. Encouragingly, pretrained tactile encoders transfer cleanly, hinting that touch foundation models can become a reusable building block for VLAs.

{% include figure.liquid loading="lazy" path="assets/img/main_results.png" class="img-fluid rounded" zoomable=true caption="Real-robot insertion experiments with a Franka Panda and a DIGIT tactile sensor. TacFiLM improves success rates and force stability over vision-only baselines, with the biggest improvements on tight-tolerance insertions." %}

 <!-- {% include results_table.liquid
      title=" In-distribution results (avg. across 4 tasks)"
      headers="Method,Success (%),Direct (%),Max Force (N),Time (s)"
      open=false
      rows=page.id_results %} -->

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
