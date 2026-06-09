---
layout: page
title: Crop Row Detection, Segmentation to Navigation
description: A hybrid perception-and-reasoning pipeline that finds the drivable corridor between corn rows from a single camera and turns it into steering output.
img: assets/img/projects/crop_pipeline_output.jpg
importance: 2
category: research
related_publications: false
---

Under a closed crop canopy, RTK-GPS drops out and an agricultural robot has to steer
from vision alone. This project is a **hybrid perception-then-reasoning pipeline** for
that problem: a **U-Net with a ResNet-34 (ImageNet) encoder** segments the drivable
inter-row corridor from a forward-facing RGB image, a transparent **five-step geometric
refinement layer** turns that mask into left, right, and centerline polynomials, and a
final stage converts the centerline into **navigation outputs** (heading angle,
curvature, and a steering decision). Built on a single-session corn-field subset of a
published row-detection corpus.

<div class="row justify-content-sm-center">
    <div class="col-sm-12 mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/projects/crop_pipeline_output.jpg" title="End-to-end pipeline output" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Left: raw input from a ground-level camera in a corn row. Right: detected row boundaries (cyan) and the centerline (yellow) the navigation stage consumes.
</div>

## At a glance

| | |
| :--- | :--- |
| **Role** | Sole author; full pipeline design, implementation, and analysis |
| **Task** | Vision-only row following under canopy, where GPS is unreliable |
| **Perception** | U-Net + ResNet-34 (ImageNet) encoder, binary corridor segmentation |
| **Reasoning** | 5-step geometric refinement → left/right/centerline polynomials |
| **Output** | Heading angle, curvature, discrete steering decision |
| **Data** | 686-frame single-session corn subset (438/110/138), 1920×1200 |
| **Compute** | Single RTX 3070 Laptop (8 GB), mixed precision, ~20 min training |
| **Results** | Mean IoU **0.82**, F1 **0.88** at native resolution; centerline error <1% of image width |

## Why it matters

Under-canopy robots are how you get plant-level data, like stem width, early disease,
and lodging, that drones overhead can't see and tractors can't reach without crushing
the crop. The blocker is navigation: once the canopy closes, GPS multipath makes it
unreliable, and repeating rows defeat the loop-closure that SLAM depends on. A camera
that estimates the corridor and hands a controller a clean centerline is a practical
way through.

## Two findings worth keeping

**The headline IoU hides a bimodal distribution.** Mean IoU is 0.82, but the median is
0.885 and a small tail of near-zero frames drags the average down. The model isn't
uniformly mediocre, it's mostly right and occasionally catastrophically wrong, almost
always by hallucinating a corridor into dense foliage (a false positive). That's the
expensive failure: a false positive steers into a crop row, while a false negative just
slows the robot down. The practical takeaway is that a deployed system needs a
confidence check, not blind trust in every mask.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/projects/crop_iou_distribution.png" title="Per-image IoU distribution" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/projects/crop_predictions_grid.jpg" title="Worst, median, and best predictions" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Left: the strongly bimodal per-image IoU distribution, the central interpretability result. Right: worst (top), median (middle), and best (bottom) test predictions against ground truth.
</div>

**The refinement layer earns its keep on the worst cases, not the average.** Ablating
the five heuristics one at a time shows they cut mean centerline error by only ~7.5%,
because the U-Net already produces clean masks most of the time. But on the hardest
frames the same layer reduces error by an order of magnitude, e.g. from 93.9 px to
8.5 px. It functions as a safety net for the long tail rather than an average-case win.

<div class="row justify-content-sm-center">
    <div class="col-sm-11 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/projects/crop_refinement_beforeafter.jpg" title="Before vs after geometric refinement" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Before/after the geometric refinement layer on the three frames it helps most: noisy raw boundaries (cyan) and centerline (yellow) snap back toward ground truth (green dashed).
</div>

## An honest limitation

All three splits come from one recording session, so they're temporally correlated and
the reported numbers will overestimate performance on a different field or season. This
is the standard domain-shift problem in row detection, and the natural next steps are
evaluation on a held-out video, a precision-biased asymmetric loss to suppress the
false-positive failures, and a per-frame confidence head so a controller can refuse
low-confidence predictions.

## What this project demonstrates

- **Deep segmentation under real constraints.** Transfer learning and architecture choices made deliberately for a small, single-video dataset on an 8 GB GPU.
- **Perception plus interpretable reasoning.** A transparent geometric layer that can be inspected, ablated, and tuned independently of the network.
- **Honest evaluation.** Distribution-level analysis that surfaces failure structure a headline metric would hide, plus explicit reasoning about asymmetric error costs.
- **Path to control.** Segmentation output carried all the way to heading, curvature, and steering decisions a row-following controller can use.
