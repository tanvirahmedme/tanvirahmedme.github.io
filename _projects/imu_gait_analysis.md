---
layout: page
title: IMU Gait Analysis, Walking vs. Running
description: A wearable dual-IMU setup that captures and compares the biomechanics of walking and running from foot-mounted motion sensors.
img: assets/img/projects/gait_sensors_feet.jpg
importance: 3
category: research
related_publications: false
---

A focused study on **wearable gait analysis**. I used two WitMotion WT901WIFI IMUs,
one on the dorsum of each foot, to record and compare the biomechanics of **walking
and running** in real time. The project covers the full signal pipeline: synchronized
motion capture streamed over Wi-Fi, cleaning and zero-lag filtering, gait-cycle
segmentation from heel-strike peaks, and statistical feature extraction that quantifies
how the two gaits differ. Published as *Human Motion Gait Analysis Using IMU Sensors*
and supported by USDA-NIFA Grant 2023-77040-41262.

<div class="row justify-content-sm-center">
    <div class="col-sm-8 mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/projects/gait_sensors_feet.jpg" title="IMU sensors mounted on both feet" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    One WitMotion WT901WIFI mounted on the dorsum of each foot, capturing 3-axis acceleration and angular velocity during treadmill trials.
</div>

## At a glance

| | |
| :--- | :--- |
| **Role** | Lead author; data collection, signal processing, and analysis |
| **Sensors** | 2× WitMotion WT901WIFI (MPU9250 9-axis IMU), foot-dorsum mounted |
| **Signals** | 3-axis linear acceleration + 3-axis angular velocity, 10 Hz, over Wi-Fi |
| **Capture** | Treadmill (Precor TRM 835), separate walking and running trials |
| **Processing** | Python/pandas cleaning, 4th-order Butterworth low-pass (4 Hz, zero-lag) |
| **Analysis** | Heel-strike peak detection, gait-cycle segmentation, statistical features |
| **Funding** | USDA-NIFA Grant 2023-77040-41262 |

## Why it matters

Clinical gait labs using multi-camera systems like Vicon and OptiTrack are accurate but
expensive, and confined to a setting that doesn't reflect how people actually move.
Foot-mounted IMUs are cheap, portable, and capture both the stance and swing phases
continuously, which makes gait screening practical outside the lab, in rehabilitation,
sports, and everyday monitoring.

## Setup and method

The dorsum was chosen as the mount point because its flexibility gives a clean,
detailed picture of foot motion through each phase. Sensors on both feet allow a
direct left/right comparison. Raw data was streamed over Wi-Fi into the WitMotion app,
exported, then cleaned in pandas down to the acceleration and gyroscope channels.
After a 4th-order Butterworth low-pass filter (4 Hz cutoff, zero-lag), I computed the
**resultant acceleration and angular velocity magnitudes** to remove orientation
dependence, then detected heel strikes as peaks in the acceleration signal to segment
individual gait cycles.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/projects/gait_cycle_phases.jpg" title="Phases of the normal gait cycle" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/projects/gait_sensor_strap.jpg" title="WT901WIFI on Velcro strap" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Left: the stance and swing phases of a normal gait cycle, the framework for interpreting the signals. Right: the WT901WIFI mounted on an adjustable Velcro strap.
</div>

## Walking vs. running

Segmenting and comparing cycles surfaces a clear biomechanical signature. Walking is
slower and more deliberate: cycles of roughly **1–1.2 s**, with acceleration peaks near
**5 m/s²** and angular velocity peaks near **500 °/s**, and well-defined troughs between
phases. Running compresses everything: cycles of **0.6–0.8 s**, sharper peaks reaching
**~6 m/s²** and **~600 °/s**, higher variability, and a right-skewed distribution
reflecting explosive, intermittent bursts of force.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/projects/gait_walking_accel.jpg" title="Linear acceleration during walking" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/projects/gait_running_accel.jpg" title="Linear acceleration during running" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Resultant linear acceleration for both feet during walking (left) and running (right). Running shows shorter intervals between heel strikes and higher peak impacts.
</div>

## What this project demonstrates

- **Wearable sensing.** Multi-sensor, dual-foot motion capture with wireless data streaming.
- **Biomechanical signal processing.** Zero-lag Butterworth filtering and orientation-independent magnitude derivation.
- **Gait-cycle analysis.** Heel-strike event detection, cycle segmentation, and statistical feature extraction.
- **Comparative kinematics.** Quantifying how walking and running differ in cycle duration, impact, and rotational dynamics.
