---
layout: page
title: Smart Cattle Collar — Wearable Sensing + ML
description: A custom low-power wearable that predicts cattle behavior from motion and GPS data — designed end-to-end from PCB to a deployed real-time ML dashboard.
img: assets/img/projects/collar_field_deployed.jpg
importance: 1
category: research
related_publications: false
---

A fully custom **wearable sensor collar** that monitors cattle and predicts their
behavior — grazing, walking, resting, and other — from on-animal motion and location
data, in real time. I built the entire system end to end: schematic and multi-layer
**PCB design**, a rugged 3D-printed **enclosure**, **embedded firmware** for sensor
acquisition and logging, the **machine learning** pipeline that classifies behavior,
and a **deployed real-time dashboard** served from a FastAPI backend. The collar
streams telemetry to a local edge server or autonomous agricultural rover, laying the
groundwork for closed-loop, autonomous livestock management. Built and field-validated
on cattle at the Texas State University Freeman Center, under USDA-NIFA Grant 2023-77040-41262.

<div class="row justify-content-sm-center">
    <div class="col-sm-12 mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/projects/collar_field_deployed.jpg" title="Collar deployed on a steer in the field" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    The collar deployed on a steer grazing in the paddock at the Freeman Center — the full system running in a real agricultural environment.
</div>

## At a glance

| | |
| :--- | :--- |
| **Role** | Sole hardware, firmware, and ML developer |
| **Hardware** | Custom 2-layer PCB (KiCad &rarr; JLCPCB), 3D-printed weatherproof enclosure |
| **Compute** | Arduino Nano 33 IoT (ARM Cortex-M0+), MPU-6050 6-axis IMU, XA1110 GNSS |
| **Firmware** | Multi-sensor acquisition @ 10 Hz, SPI microSD logging, RTC time-stamping |
| **ML** | Random Forest, XGBoost, 1D-CNN, BiLSTM, CNN&ndash;BiLSTM (focal loss) |
| **Best model** | Random Forest + FFT features + SMOTE &mdash; **86.2% accuracy** |
| **Deployment** | Real-time prediction dashboard served via FastAPI |
| **Connectivity** | u-blox NINA-W10 Wi-Fi/BLE &mdash; streams telemetry to edge server / rover |
| **Funding** | USDA-NIFA Grant 2023-77040-41262 |

## Why it matters

Behavioral changes are among the earliest signals of illness, estrus, or distress in
cattle — but spotting them by eye across a herd is labor-intensive and often too late
to act on. A wearable that continuously senses motion and classifies behavior turns
that into an automated, always-on signal. The longer-term vision is closed-loop: the
collar streams behavior and location to an autonomous rover that can investigate or
intervene when something looks wrong. The hard part is doing it cheaply, ruggedly, and
at low enough power to survive in the field — which meant building custom hardware
rather than buying it.

## Hardware: from schematic to fabricated board

I designed the collar's electronics from scratch in KiCad — schematic capture,
electrical rule checks, footprint assignment, and a double-layer PCB layout with
dedicated net classes for power vs. signal traces, ground planes for EMI control, and
impedance-conscious routing. Gerbers were fabricated by JLCPCB and hand-assembled.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/projects/pcb_layout_kicad.jpg" title="KiCad PCB layout" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/projects/pcb_fabricated.jpg" title="Fabricated PCB" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/projects/pcb_assembled.jpg" title="Assembled PCB" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Left to right: the 2-layer board routed in KiCad, the fabricated bare board (front and back), and the fully soldered, powered, and tested unit.
</div>

The second-generation board was a substantial redesign: a **30.4% smaller footprint**
and roughly **59% less volume** than version 1, achieved by integrating the IMU into
the microcontroller, switching from a cylindrical to a flat Li-Po cell, and replacing
an unreliable 433 MHz radio with onboard Wi-Fi/Bluetooth — all while *increasing*
battery capacity.

<div class="row justify-content-sm-center">
    <div class="col-sm-10 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/projects/version_comparison.jpg" title="Version 1 vs Version 2" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Version 1 (left) vs. the redesigned Version 2 (right): smaller, thinner, more reliable, and higher capacity.
</div>

## Enclosure: rugged, weatherproof, animal-friendly

The electronics live in a custom clamshell enclosure I designed for field durability
— a gasket-sealed, impact-resistant housing with integrated belt loops, a latch-and-hinge
system, and internal ribbing tuned for 3D printing. It was printed in PLA with
dissolvable PVA supports on an Ultimaker S5 Pro.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/projects/enclosure_exploded.jpg" title="Enclosure design breakdown" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/projects/enclosure_printed_open.jpg" title="3D-printed enclosure with electronics" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/projects/collar_assembled.jpg" title="Final assembled collar" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    The annotated enclosure design (latch, belt loop, rubber seal, mounting points), the printed housing with the board seated inside, and the finished collar on a high-visibility nylon strap with a side-release buckle.
</div>

## Field deployment & data collection

The collar was deployed on a steer at the Freeman Center across three multi-hour
sessions, logging IMU and GNSS data at 10 Hz to onboard storage. Two time-synced
cameras recorded the paddock so every sensor window could be matched against video
ground truth for behavior labeling — yielding roughly **395,000 raw samples** hand-labeled
into four behavior classes.

<div class="row justify-content-sm-center">
    <div class="col-sm-7 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/projects/field_site.jpg" title="Freeman Center field site" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    The ~270 m paddock at the Freeman Center used for the trials, mapped for camera coverage and synchronized with the collar's timestamps.
</div>

## Machine learning

The data is heavily **imbalanced** — grazing dominates at 65%, while walking is under
3% — which is the central modeling challenge: a naive classifier scores high accuracy
by ignoring the rare-but-important behaviors. Handling that imbalance honestly drove
most of the ML work.

<div class="row justify-content-sm-center">
    <div class="col-sm-6 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/projects/behavior_distribution.jpg" title="Behavior class distribution" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm-6 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/projects/feature_importance.jpg" title="Random Forest feature importances" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Left: the real class imbalance across the four behaviors. Right: top feature importances from the deployed Random Forest, blending time-domain statistics with FFT frequency-domain features.
</div>

**Pipeline.** Raw signals are segmented into 50-sample (10-second) windows, then
expanded with time-domain statistics and **FFT-based frequency-domain features**.
To combat imbalance I used **SMOTE** oversampling, class-weight balancing, and
`RandomizedSearchCV` tuned on macro-F1, with early stopping on the deep models.

**Models.** I benchmarked classical models (Random Forest, XGBoost) against deep
sequence models (1D-CNN, BiLSTM, and a CNN&ndash;BiLSTM hybrid with weighted focal loss).
The deep models with focal loss recovered more of the rare classes but traded away
overall accuracy and added training complexity. The **Random Forest with FFT features
and SMOTE won on the accuracy/practicality trade-off — 86.2% accuracy** with the best
macro-F1 among balanced configurations — so it was the one I deployed.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/projects/confusion_matrix_rf.jpg" title="Random Forest confusion matrix" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/projects/confusion_matrix_cnnbilstm.jpg" title="CNN-BiLSTM confusion matrix" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Confusion matrices for the two best models: the deployed Random Forest (left) and the CNN&ndash;BiLSTM hybrid with weighted focal loss (right), which traded accuracy for stronger recall on minority behaviors.
</div>

## Deployment: real-time dashboard

The selected model runs behind a **FastAPI** server that ingests the collar's live
telemetry and serves a real-time dashboard — plotting the animal's GPS location on a
map, the current predicted behavior, and the raw sensor feed side by side. This closes
the loop from a bare schematic all the way to a running inference service.

<div class="row justify-content-sm-center">
    <div class="col-sm-11 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/projects/ml_dashboard.jpg" title="Real-time cattle collar dashboard" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    The live dashboard: GPS tracking, real-time behavior prediction, system status, and a streaming telemetry table — powered by the deployed Random Forest model.
</div>

## What this project demonstrates

- **Full-stack hardware ownership** — schematic, PCB layout, fabrication, and bring-up of a custom mixed-signal board
- **Mechanical design for the real world** — parametric CAD and additive manufacturing for a sealed, field-rugged enclosure
- **Embedded systems** — multi-sensor acquisition, SPI storage, power management, and battery-life optimization
- **Applied ML on messy, imbalanced real data** — sensor fusion, FFT feature engineering, SMOTE/focal-loss imbalance handling, and classical-vs-deep benchmarking
- **Deployment** — packaging the winning model into a live FastAPI inference service with a real-time dashboard
- **End-to-end execution** — from a blank schematic to a deployed system classifying behavior on a live animal in the field
