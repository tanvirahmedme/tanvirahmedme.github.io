---
layout: page
title: 3D-Printable Bonded Magnetic Composite
description: A strontium-ferrite / polyamide 4.6 composite filament made by twin-screw extrusion for fused-filament 3D printing of magnetic parts, characterized for microstructure, thermal stability, and magnetic anisotropy.
img: assets/img/projects/magnet_twin_screw_extruder.png
importance: 4
category: research
related_publications: false
---

A co-authored study on fabricating a **bonded magnetic composite** that can be 3D
printed. Anisotropic strontium-ferrite powder is compounded into a **polyamide 4.6**
binder by twin-screw extrusion to produce a 1.75 mm monofilament suitable for
fused-filament fabrication, then characterized across microstructure, thermal behavior,
and magnetic performance. The motivation is a cheaper, more flexible route to magnetic
components, like small motors, generators, and Halbach arrays for MRI, than the
expensive tooling that injection-molded or sintered magnets require.

> **My role:** *co-author — set this to your actual contribution (e.g. magnetic / VSM characterization, data analysis, manuscript). I left it deliberately blank rather than guess.*

<div class="row justify-content-sm-center">
    <div class="col-sm-12 mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/projects/magnet_twin_screw_extruder.png" title="Twin-screw extrusion setup" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    The twin-screw compounding line: polyamide 4.6 pellets and strontium-ferrite powder are fed, melt-compounded across eight heating zones (290–330 °C), and drawn into a 1.75 mm filament for FFF printing.
</div>

## At a glance

| | |
| :--- | :--- |
| **Materials** | Anisotropic strontium-ferrite (hexaferrite) powder + polyamide 4.6 binder |
| **Loadings** | 20 wt% and 40 wt% magnetic filler |
| **Process** | Twin-screw compounding → 1.75 ± 0.05 mm monofilament for FFF 3D printing |
| **Characterization** | SEM (microstructure), TGA + DSC (thermal), VSM (magnetic) |
| **Key result** | Flow-induced magnetic anisotropy with an easy axis perpendicular to the extrusion direction |
| **Application** | Feedstock for magnetic-field-assisted additive manufacturing of magnetic devices |

## What the characterization showed

**Microstructure.** SEM confirmed the ferrite platelets stayed evenly dispersed in the
nylon matrix with no appreciable agglomeration at either loading, which matters because
clustering would form complex magnetic domains and weaken the composite's magnetic
response.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/projects/magnet_sem_microstructure.png" title="SEM of the bonded composite" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/projects/magnet_tga.png" title="Thermogravimetry plot" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Left: SEM of the 40 wt% composite showing well-dispersed ferrite platelets. Right: TGA mass-loss curves, which put the measured filler fractions (21.9% and 40.0%) within ±3% of target.
</div>

**Thermal behavior.** The polyamide 4.6 melting point (~290 °C) was essentially
unchanged by adding filler, so the composite stays printable and thermally robust.
Crystallinity rose with loading (70.5% → 79.9% → 88% for neat, 20 wt%, and 40 wt%),
consistent with the ferrite particles acting as nucleation sites. TGA also showed
measured filler fractions within ±3% of target, evidence the extrusion process was
well optimized.

**Magnetic performance.** Field-angle hysteresis measurements revealed **flow-induced
anisotropy**: the easy axis (where magnetization is most favorable) lies perpendicular
to the extrusion direction, attributed to shear flow aligning the platelets near the
extruder nozzle. The S-value (squareness, Mr/Ms) peaks at the 90° and 270° field
angles, confirming the two easy directions. This is the practically useful finding,
because printing this filament in an applied magnetic field could lock in and amplify
that alignment to make stronger magnetic parts.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/projects/magnet_hysteresis_loop.png" title="Field-angle hysteresis loops" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/projects/magnet_svalues_anisotropy.png" title="S-values vs field angle" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Left: normalized hysteresis loops across field angles for the 20 wt% composite. Right: S-values (Mr/Ms) vs field angle, peaking near 90°/270° and marking the easy axis.
</div>

## What this project demonstrates

- **Composite fabrication.** Melt-compounding a ceramic magnetic filler into a thermoplastic binder to make printable feedstock.
- **Multi-technique characterization.** SEM, TGA/DSC, and VSM combined to connect microstructure, thermal stability, and magnetic behavior.
- **Process–structure–property reasoning.** Linking extrusion shear flow to platelet alignment, and that alignment to measurable magnetic anisotropy.
