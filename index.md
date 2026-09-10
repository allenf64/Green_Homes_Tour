---
layout: default
title: Home
---

## One year of solar production, checked against reality

This page walks through a year of daily output from a 14-panel, 6.02 kW
rooftop solar array in Denver, Colorado — checked against two independent
sources of truth: the physics of where the sun actually was each day, and
NASA's satellite-measured weather for that same period.

### The short version

- **Performance ratio: 74.3%** — a normal, healthy number for a real system.
- **18 days** underperformed even after accounting for that day's actual weather.
- Several of those days land the day *after* a heavy storm — a likely sign of
  snow sitting on the panels rather than a hardware problem.

The full interactive dashboard is embedded further down this page. Here's
how it was built.

---

## How the analysis was built, step by step — in plain English

You gave me one year of daily solar production numbers and asked how much of
that was normal weather versus something worth checking on the roof. Here's
exactly what was looked at, what was built, and what it showed — including
the mistakes that were caught and fixed along the way.

### 1. What data was actually used

- **Daily energy report** (an `.xls` file exported from the monitoring
  system) — 366 days of data, September 8, 2025 through September 8, 2026,
  with one number per day: how many kilowatt-hours (kWh) the system produced
  that day. This is the "actual" side of every comparison in this report.
- **A panel layout drawing and a photo of the house** — a diagram showing
  every physical panel: its brand (SilFab or Ureco), its wattage (410 W or
  445 W), and which direction it faces (south-sloped roof, west roof, east
  roof, or a vertical wall). This gave the exact shape and size of the
  system instead of a rough guess.
- **The datasheet for the microinverter** (APsystems DS3-L), found online —
  it showed that each inverter tracks two panels independently, meaning each
  panel's output could be modeled on its own instead of assuming the whole
  array behaves like one block.
- **NASA's POWER weather dataset** (a CSV downloaded from NASA's public data
  tool at [power.larc.nasa.gov](https://power.larc.nasa.gov)) — this gave two
  numbers for every day: how much sunlight actually reached the ground
  (accounting for real clouds and storms), and how much sunlight would have
  reached the ground on a perfectly clear day. Comparing those two shows
  exactly how cloudy each day really was.

### 2. How the "what should this array have produced" model was built

To know whether a day's output was "low," a fair prediction of what that
day's output *should* have been was needed first — given the sun's
position, the season, and the panels' physical orientation. This was built
using an open-source physics library called `pvlib`, which implements the
same published equations used by the U.S. government's own solar
calculator, PVWatts.

- For every 15 minutes of every day of the year, the model calculates
  exactly where the sun is in the sky above Denver (its height and compass
  direction).
- It then calculates how much sunlight would hit each of the four panel
  groups (south roof, west roof, east roof, and the vertical wall) at that
  exact moment, given each one's specific tilt and compass direction.
- It converts that sunlight into expected electrical output, using each
  group's real wattage (for example, the south roof's 7 panels totaling
  3,010 watts), and adjusts slightly for how heat affects panel efficiency.
- Adding up all four groups, for every day of the year, gives one number:
  the maximum the system could have produced that day under a perfectly
  clear sky — the **clear-sky ceiling**.

### 3. Checking the work along the way

The first version of this model used a guess — a single 6 kW system facing
east-or-west — because that was all the information available at the time.
As better information came in, the model was rebuilt three separate times,
using the same built-in reality check each time to know whether the new
numbers were trustworthy.

- **The reality check:** actual output can never be higher than the
  clear-sky ceiling. If the real system ever "beat" a perfectly sunny day in
  the model, that's not possible in real life — it means the model's numbers
  are wrong, most likely because the assumed system size is off.
- **Round 1 (the drawing):** the layout drawing showed 9 panels totaling
  3.87 kW. Plugging that into the model, actual output exceeded the
  clear-sky ceiling on 214 of 366 days — a clear sign the panel count was
  too low.
- **Round 2 (first correction):** correcting the west and east roof counts
  and the south roof's Ureco count brought the system to 13 panels / 5.61
  kW. That dropped the failures from 214 days to zero — a great sign — but
  the total was then found to actually be 14 panels, not 13.
- **Round 3 (final correction):** adding the missing south-roof SilFab panel
  brought the system to its final, correct size: 14 panels, 6.02 kW. The
  reality check still passed (zero days exceeded the ceiling), and —
  importantly — the same 14 suspicious low-output days kept showing up no
  matter which version of the model was used. That consistency confirmed
  those specific days were a real signal, not a modeling mistake.

### 4. Adding real weather, not just an idealized sunny day

A "clear-sky ceiling" assumes zero clouds, every single day — which never
actually happens. That made every cloudy day look like a suspicious "low"
day, even when clouds were the only real cause. NASA's weather data fixed
this.

- For every day, a **clearness index** was calculated: the actual measured
  sunlight divided by the clear-sky sunlight for that same day. A value near
  1.0 means it was nearly cloudless; a value near 0.2 means it was heavily
  overcast.
- Each day's clear-sky ceiling was then multiplied by that day's clearness
  index, producing a new, realistic expected output that already accounts
  for that day's real weather.
- The result is called a **performance ratio**: actual output divided by
  this weather-adjusted expected output. This is the standard metric the
  solar industry itself uses to judge how well a system is performing,
  independent of weather.

### 5. What the analysis found

- **Overall performance ratio: 74.3%.** The system delivered about
  three-quarters of what it should have, given the actual weather all year.
  That's a completely normal, healthy number — typical well-functioning home
  solar systems run 75–85%.
- **18 days stood out as underperforming** even after accounting for that
  day's real weather — meaning something beyond ordinary clouds was likely
  reducing output.
- **3 days that looked suspicious at first turned out to be nothing:**
  December 5, May 5, and May 18 all had genuinely cloudy weather that fully
  explains their low output.
- **7 new suspicious days were uncovered** that hadn't shown up before,
  because the weather that day looked fine on paper but output was still
  unusually low: November 29, December 4, December 11, January 1, March 7,
  May 7, and July 27.
- **The most interesting pattern:** several flagged days land the day
  immediately after a heavy storm (December 3 into December 4; March 6 into
  March 7). The storm day itself is expected to be low — it was cloudy — but
  the day right after, when skies had cleared, output stayed low anyway.
  That's the signature of snow sitting on the panels rather than clouds
  blocking the sun: the sky clears before the panels do.
- **Suggested next step:** checking Denver's historical snowfall records
  against these specific dates would confirm whether snow cover, rather than
  a hardware issue, explains the pattern — that data wasn't part of this
  analysis and would need to be pulled separately.

---

## The interactive dashboard

Hover over the charts below to see exact values for any day. If it looks
cramped on your screen, use the link underneath to open it full-page.

<iframe src="dashboard.html" style="width:100%; height:1400px; border:1px solid #ddd; border-radius:8px;" loading="lazy"></iframe>

[Open the dashboard in its own tab →](dashboard.html){:target="_blank"}

---

## Sources & references

- **Daily production data** — exported from the site owner's solar
  monitoring platform (APsystems EMA), covering September 8, 2025 through
  September 8, 2026.
- **Panel layout** — hand-drawn diagram and site photo provided by the site
  owner, listing panel brand, wattage, and roof orientation for all 14
  panels.
- **Microinverter specifications** — [APsystems DS3-L Series Datasheet](https://global.apsystems.com/document/apsystems-ds3-series-datasheet/), APsystems.
- **Solar position and output modeling** — [pvlib python](https://pvlib-python.readthedocs.io/), an open-source library co-developed with NREL/Sandia National Laboratories that implements the published PVWatts equations. Holmgren, W., Hansen, C., & Mikofski, M. (2018). pvlib python: a python package for modeling solar energy systems. *Journal of Open Source Software*, 3(29), 884.
- **PVWatts model reference** — [PVWatts Calculator](https://pvwatts.nrel.gov/), National Renewable Energy Laboratory (NREL), U.S. Department of Energy.
- **Historical weather / irradiance data** — [NASA POWER (Prediction Of Worldwide Energy Resources) Project](https://power.larc.nasa.gov/), NASA Langley Research Center, daily `ALLSKY_SFC_SW_DWN` and `CLRSKY_SFC_SW_DWN` parameters.
- **Site coordinates** — approximate location, Denver, Colorado metro area.

## Authorship

This analysis and write-up were produced collaboratively by the site owner
and **Claude (Claude Sonnet 5)**, an AI model developed by
[Anthropic](https://www.anthropic.com), acting as co-author — handling data
processing, physical modeling, statistical analysis, visualization, and
drafting, under the direction and domain knowledge (panel layout, system
specifics, and corrections) of the site owner.

*Last updated: September 2026.*
