---
layout: post
title: "A Perovskite Solar Cell on Ceramic Has Been Running Outdoors for Over a Year"
date: 2026-03-16
tags: [perovskite, outdoor-testing, BIPV, ParaSol, Perovskino, ceramic,research, BONIFACE, outdoor-stability]
---

In December 2024, we installed two solar cells on our [ParaSol](https://www.emiliojuarez.es/2025/02/01/OSSLab/) outdoor testing platform on the rooftop of the INMA building in Zaragoza: a conventional silicon reference cell and a perovskite cell fabricated on porcelain stoneware — the same ceramic material widely used in building facades and floors.

The question was straightforward: can a perovskite solar cell on a construction-grade ceramic substrate survive real outdoor conditions?

As of today, the answer is yes — for over 430 days and counting.

## What the cell has endured

Zaragoza's continental climate is not kind to solar devices. Over the course of more than 14 months of continuous outdoor operation, the cell has been exposed to:

- Device surface temperatures exceeding **60 °C** during summer
- Relative humidity swings from **40% to over 90%**
- Cumulative solar irradiation above **2000 kWh/m²**
- Rain, wind, dust, and the full cycle of seasons from winter to winter

The figure below shows the daily efficiency of both the silicon reference cell (left) and the perovskite-on-ceramic cell (right), together with the temperature and humidity conditions recorded throughout the entire period.

![Figure 1: Stability data for silicon and perovskite-on-ceramic cells over 430+ days](/imgs/BONIFACE_1year_figure.png){:style="display:block; margin-left:auto; margin-right:auto"}
*Figure 1: Stability data for silicon and perovskite-on-ceramic cells over 430+ days.*
{: style="color:gray; font-size: 70%; text-align: center;"}

## Why this matters

Perovskite solar cells are one of the most promising photovoltaic technologies: they can be fabricated at much lower temperatures than silicon (which requires processes well above 1000 °C), with significantly lower energy consumption and carbon footprint. However, their long-term stability under real outdoor conditions has been their biggest question mark.

Most published stability data comes from laboratory testing — controlled environments, accelerated aging protocols, or encapsulated devices under idealized conditions. Real outdoor data, measured day after day through actual weather, is still scarce. This result contributes to filling that gap.

## Why ceramics?

Porcelain stoneware is one of the most widely used materials in construction — facades, ventilated walls, rooftops, and floors. It is durable, weather-resistant, and produced by a well-established industrial sector, particularly strong in Spain.

If perovskite photovoltaic technology can be reliably integrated into ceramic substrates, it opens a path toward **building-integrated photovoltaics (BIPV)** at scale: building envelopes that generate electricity without requiring dedicated panel installations. Imagine facades and rooftops that are both architectural elements and power generators.

## How we track it

All data is collected continuously using [Perovskino](https://www.emiliojuarez.es/2025/02/01/OSSLab/), our open-source Arduino-based galvanostatic maximum power point tracker, specifically designed for perovskite solar cells. Perovskino addresses the hysteresis effects that make conventional MPPT algorithms unreliable for perovskite devices. The design has been published in [Cell Reports Physical Science](https://doi.org/10.1016/j.xcrp.2024.101856) and [STAR Protocols](https://doi.org/10.1016/j.xpro.2024.102953).

The ParaSol platform integrates multiple Perovskino units with environmental sensors (temperature, humidity, irradiance, wind) and automated data pipelines for continuous monitoring.

## What's next

We continue working to improve both efficiency and durability. This milestone — over one year of continuous outdoor operation — is an important step, but the road to commercial viability requires much longer demonstration periods and further optimization.

We are also expanding collaborations with international groups to replicate outdoor testing in different climates, which will be essential to validate perovskite stability across a range of real-world conditions.

---

[OSS Lab Telegram channel](https://t.me/oss_lab)

*This work is part of the **BONIFACE** project (CPP2022-009766), a public-private collaboration between the [Institute of Nanoscience and Materials of Aragón](https://inma.unizar.es/) (INMA, CSIC – University of Zaragoza) and [Gres Aragón](https://www.gresaragon.com/) (SAMCA Group), funded by MCIN/AEI/10.13039/501100011033 and by the European Union — NextGenerationEU/PRTR.*
