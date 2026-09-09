# App Store and Google Play — profiling app categories by user numbers

Which categories of free, English-language apps attract the most users on Google Play
and the iOS App Store, from the point of view of a developer whose revenue comes from
in-app ads. The number of users is the thing that matters, so the analysis ranks
categories by average installs (Android) and average rating count as a proxy for
installs (iOS), and then checks whether those averages are being driven by a handful
of outliers.

Written in plain Python with the standard library only, no pandas, no NumPy.

## Data

| File | Rows | Source |
| --- | --- | --- |
| `googleplaystore.csv` | 10,841 | [Google Play Store Apps](https://www.kaggle.com/datasets/lava18/google-play-store-apps), Lavanya Gupta (Kaggle, 2018) |
| `AppleStore.csv` | 7,197 | [Mobile App Store (7200 apps)](https://www.kaggle.com/datasets/ramamet4/app-store-apple-data-set-10k-apps), Ramanathan Perumal (Kaggle, 2018) |

Both are from 2018, so the findings describe the market as it was then, this may have changed with the introduction of viral apps such as TikTok.

## Cleaning

1. Removed one malformed Google Play row with a shifted column (row 10472).
2. Removed 1,181 duplicate Google Play entries, keeping the row with the highest
   review count for each app on the assumption that it is the most recent snapshot.
   10,840 → 9,659.
3. Removed non-English apps by counting characters outside ASCII 0–127. A strict
   filter drops legitimate apps containing `™` or emoji, so the threshold is set at
   more than three non-ASCII characters. This still misclassifies some apps in both
   directions.
4. Kept only free apps, since the business model is ad revenue.

Final sets: 8,864 Android apps and 3,222 iOS apps.

| Step | Android | iOS |
| --- | --- | --- |
| Raw | 10,841 | 7,197 |
| Deduplicated | 9,659 | — |
| English | 9,614 | 6,183 |
| Free | 8,864 | 3,222 |

## Findings

**iOS.** Games make up around 58% of free English apps, with every other genre below
10%. Share of apps is not the same as share of users: by average number of ratings,
Navigation and Reference come out on top, but Navigation is almost entirely Waze and
Google Maps, so the average is meaningless. Reference is more evenly distributed
across apps and is the more genuine result.

**Android.** The category mix is different as Family and Tools are large, and games
aimed at children sit under Family rather than Game, so the Google Play catalogue
skews more practical than the App Store. By average installs, Communication leads at
38.5m, but dropping apps above 100m installs (WhatsApp, Messenger, Skype, Gmail and
similar) takes the average to 3.6m, a fall of roughly ten times. Video Players and
Social have the same problem.

**Both stores.** Books and Reference is the category that looks workable on both
sides. It is skewed by a few large apps, but the mid-range — 1m to 50m installs — is
varied rather than dominated by one product type. Much of it is ebook readers and
processors, which is a crowded space, but a noticeable number of apps are built
around a single popular book, mostly the Quran. Taking one popular book and building
an app around it looks viable on both stores, though a plain text version would not
be enough to compete with the existing libraries: daily quotes, audio, quizzes and a
discussion forum would be the kind of additions needed.

## Running it

```
git clone https://github.com/<your-username>/app-store-analysis.git
cd app-store-analysis
jupyter notebook AppsAnalysis.ipynb
```

Python 3 and Jupyter are the only requirements. The notebook reads both CSVs from the
repository root and runs top to bottom.

## Files

```
AppsAnalysis.ipynb    analysis notebook
googleplaystore.csv   Google Play data
AppleStore.csv        App Store data
```
