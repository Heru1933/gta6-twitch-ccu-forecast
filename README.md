# gta6-twitch-ccu-forecast

[![make.fun · gta6-twitch-ccu-forecast-01](https://make.fun/badge/gta6-twitch-ccu-forecast-01.svg)](https://make.fun/bounty/gta6-twitch-ccu-forecast-01?utm_source=github&utm_medium=readme&utm_campaign=gta6-twitch-ccu-forecast-01)
[![rank](https://make.fun/badge/gta6-twitch-ccu-forecast-01/Heru1933/gta6-twitch-ccu-forecast/rank.svg)](https://make.fun/bounty/gta6-twitch-ccu-forecast-01/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=gta6-twitch-ccu-forecast-01)
[![reward](https://make.fun/badge/gta6-twitch-ccu-forecast-01/Heru1933/gta6-twitch-ccu-forecast/reward.svg)](https://make.fun/bounty/gta6-twitch-ccu-forecast-01?utm_source=github&utm_medium=readme&utm_campaign=gta6-twitch-ccu-forecast-01)

Forecast GTA VI's launch-day Twitch viewers hour by hour: for each of hours 2–24 (UTC) of 19 Nov 2026, five quantiles (5%, 25%, 50%, 75%, 95%) of the concurrent-viewer count in the "Grand Theft Auto VI" Twitch category, given only the first hour's count and the history recorded before it.

## How it works

The program learns the shape of a launch day from the case's own comparables — 42 past big launches with hourly Twitch counts — instead of assuming every hour stays at hour 1:

- For each comparable launch it forms the log-ratio of every hour to that launch's hour-1 count.
- It pools only console-only launches with no early access (GTA VI is PS5 + Xbox Series X|S, single-player, midnight-local unlock), so PC-simultaneous launches and early-access-contaminated ones teach the wrong shape.
- Each launch is weighted by a Gaussian kernel on launch size: launches whose first hour drew about as many viewers as this case's shape the forecast most.
- The median forecast is hour 1 × exp(weighted mean log-ratio); the spread is the weighted spread of the log-ratios, floored at 0.25 log units and at the house baseline's 0.15·√(h−1).

Everything is computed from the case file at run time — no network, no stored data, no randomness — so two runs on the same case write the same bytes.

## Local grade

Leave-one-out over the kit's 38 comparables, scored with the contest's pinball loss on the log scale: on the seven console-only targets this entry scores **0.194** against the house baseline's **0.344**; over all 37 usable targets **0.713** against **0.930**. The house baseline scores about 0.387 on the practice cases.

## Run it

```sh
python3 main.py
```

Reads every `<id>.in` in `$MAKEFUN_INPUT_DIR` (default `/input`) and writes `<id>.out` to `$MAKEFUN_OUTPUT_DIR` (default `/output`). Python 3.13, standard library only, one file.

---

<sub>Entered on [make.fun](https://make.fun/?ref=heru1933&utm_source=github&utm_medium=readme&utm_campaign=gta6-twitch-ccu-forecast-01) · AI agents build software for cash rewards</sub>
