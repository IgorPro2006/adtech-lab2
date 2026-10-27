# Lab 2 — What Did the Campaign Deliver?

**Name:** Protoriev Igor
**Group:** KM-41
**Course:** Fundamentals of Ad Tech, Data \& Analytics · Fall 2026

What did a 300-spot TV campaign for a snack brand (BRAND\_A) deliver in October 2024, what did it cost, and how far can the numbers be trusted? The analysis uses the course's synthetic people-meter panel, the ad log (`commercials`) and the networks' rate cards (`rate\_card`).

## Contents

|File|What it is|
|-|-|
|`lab2.ipynb`|The full solution: ad log and ad load, pricing from the rate card, delivery (impressions, GRPs, reach, frequency) for P2+ and A18–49, CPM / CPP and the guarantee, reach overlap between big and small networks, sampling error and zero-cells, bonus reallocation. Outputs are saved.|
|`README.md`|This file.|

The data files are **not** committed: the first cells of the notebook download the course dataset
(`shkarupylomaxim/adtech-lab1-tv-panel`, version 3) with `kagglehub`.

## How to run

1. Open `lab2.ipynb` in Google Colab or Kaggle.
2. **Runtime → Run all** (the notebook runs top to bottom in about a minute).
3. If `kagglehub` asks for credentials, run `kagglehub.login()` once and retry.

## Method in short

* **Saw a spot** = the person was watching that network at the spot's `start\_ts` (session start ≤ spot start < session end). Persons are R and L only.
* **Impressions** of a spot = sum of the daily weights of the people who saw it; **rating** = impressions ÷ weights of everyone in-tab that broadcast day in the same audience × 100; **GRPs** = sum of the ratings.
* **Reach** and the frequency distribution use one monthly weight per person (the average of their daily weights); **average frequency** = GRPs ÷ reach.
* **Price** of a spot: a program row of the rate card wins; otherwise the time row that matches the broadcast weekday and the minutes since 06:00 (spot start − 6 h). A :15 costs half of the :30 price, a :60 twice.
* **CPM** = cost ÷ impressions × 1000; **CPP** = cost ÷ GRPs.
* **Standard error** of a spot's P2+ rating: √(p(1 − p) / n) × 100, with n the persons in-tab that broadcast day (a lower bound, because the panel is weighted).

## Notebook structure

`Setup` → `Task 1` ad log, ad load, pricing → `Task 2` delivery, frequency distribution, target share →
`Task 3` cost, by network, guarantee → `Task 4` do networks add up? → `Task 5` how sure are we? → `Bonus`.

