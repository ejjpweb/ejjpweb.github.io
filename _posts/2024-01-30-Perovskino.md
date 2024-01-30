---
layout: post
title: The Perovskino 
date: '2024-01-30T10:35:10+02:00'
tags: []

---

### Innovating Solar Cell Stability Assessment: The Perovskino Gizmo

In the realm of emerging solar photovoltaic technologies, assessing the operational stability of cells at the laboratory level presents a significant challenge. This challenge stems from the need for specialized equipment dedicated solely to this task over extended periods. Currently, to obtain representative data on the operational stability of a perovskite cell, continuous monitoring of its maximum power point is required for a time interval ranging from 500 to 1000 hours, as commonly reported (Figure 1).


| ![](/imgs/stability.png)  |
|:--:|
|Figure 1: *T80 (hours) vs Publication date of stability tests on Perovskite Solar Cells. Source: 1. [The Perovskite Databse Project](https://www.perovskitedatabase.com/); 2. [Materials Zone](https://app.materials.zone/apps/perovskite-database-project/stability); 3. [An open-access database and analysis tool for perovskite solar cells based on the FAIR data principles (2022)](https://doi.org/10.1038/s41560-021-00941-3)*|

The evaluation process involves the exclusive use of at least one potentiostat and a solar simulator for each device under study. While a solar simulator can illuminate multiple devices simultaneously, each typically requires a potentiostat to apply voltage and measure current at the cell terminals to determine its maximum power.

This equipment limitation has resulted in very few laboratories worldwide specializing in emerging photovoltaic research having the appropriate resources to conduct a comprehensive analysis of cell stability across a sufficient number of cells. This difficulty hampers the attainment of statistically significant results supporting improvements in stability regarding design and/or composition of active layers.

In summary, much more exhaustive research is needed in the area of operational stability of these devices. However, conducting tests on multiple cells over extended periods, coupled with the expensive necessary equipment, poses a significant challenge to progress in this field.

Unlike the assessment of simple cell efficiency with a current-voltage (JV) curve, which can be done quickly, economically, and automatedly, measuring operational stability is inherently a slow and costly process due to the required standard equipment. Additionally, it is complicated due to the specific characteristics of perovskite cells that cannot be evaluated with conventional algorithms used in traditional photovoltaics, where JV curves do not exhibit hysteresis issues.

This context helps understand why, despite advances in efficiency, the field of operational stability in perovskite solar cells has not progressed at the same level. This aspect of operational stability is crucial for the commercial development of emerging perovskite-based photovoltaics.

The [EASI project](https://easi.unizar.es) adopted an innovative approach to address this issue, the development of the "Perovskino" gizmo. 

The "Perovskino" is a maximum power point tracker for photovoltaic cells designed with a maximum cost reduction approach. Their function will be crucial for effectively and economically tracking the performance of perovskite photovoltaic cells over a long period.

#### The Low-Cost MPP Tracker "Perovskino"
"Perovskino" is a low-cost hardware (less than 5€ per unit) for long-term operational stability measurements in perovskite solar cells. Our research focuses on developing an innovative hardware solution for research purposes that allows a high number of simultaneous long-term stability measurements, eliminating the need for expensive and complex monitoring systems. Our hardware design is based on the Arduino platform, and specifically, our development is a shield that attaches to these Arduino UNO R3 Boards (Figure 2).

| ![](/imgs/perovskino.png)  |
|:--:|
|Figure 2: *The Perovskino*|



Due to the peculiarity of galvanostatically measuring cell efficiency, along with the pronounced hysteresis exhibited by perovskite devices, the "Perovskino" features a novel maximum power point tracking (MPPT) algorithm, the firmware of which we have deposited in the [GitHub repository](https://github.com/ej-jp/perovskino). Our galvanostatic MPPT algorithm ensures continuous and accurate tracking of cells with hysteresis. All work related to the development of this device and its code has been deposited as a preprint on arXiv [Enhanced Power Point Tracking for High Hysteresis Perovskite Solar Cells: A Galvanostatic Approach](https://arxiv.org/abs/2312.03124) and submitted to a peer-reviewed journal.

Stay tuned for updates on the groundbreaking innovations from the Perovskino gizmo, as we strive to revolutionize the assessment of operational stability in perovskite solar cells and drive forward the commercialization of this promising technology.







