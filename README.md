# Spotify Song Popularity Prediction

Predicting a song's Spotify popularity score (0-100) from its audio features and genre.

## Dataset

114,000 tracks from Spotify, 21 columns including audio features (danceability, energy,
loudness, acousticness, valence, tempo, etc.), genre, and a popularity score.
Source: "Spotify Tracks Dataset" (Kaggle - maharshipandya).

## Question

Can audio characteristics of a track predict how popular it is?

## My Approach

1. **EDA (Exploratory Data Analysis)** - checked missing values (negligible, 3 rows across metadata columns, dropped),
   plotted the popularity distribution, and computed correlations between each audio
   feature and popularity.
2. **Key finding from EDA** - every individual audio feature has near-zero linear
   correlation with popularity (max |r| ≈ 0.10). Genre, however, has a large effect:
   average popularity ranges from 59 (pop-film) down to 2 across genres.
   This suggested the signal, if it existed, wasn't linear or wasn't in single features
   alone.
3. **Baseline model** - linear regression on audio features + one-hot encoded genre.
4. **Improved model** - random forest, to test whether the signal was there but non-linear
   (interactions between features) rather than absent.

## Results

| Model             | RMSE  | MAE   | R²    |
|-------------------|-------|-------|-------|
| Linear Regression | 19.12 | 14.09 | 0.259 |
| Random Forest     | 15.28 | 10.32 | 0.527 |

Random forest roughly doubled R² over the linear baseline, confirming the EDA hypothesis:
the signal was there, just non-linear. Linear regression can't model feature interactions;
random forest can.

## Feature importance

No single feature dominates, importances are clustered fairly evenly across ~10 audio
features (duration, speechiness, acousticness, valence, tempo, danceability, loudness,
energy, liveness, instrumentalness all in the 0.05-0.07 range). This means popularity is
driven by combinations of weak signals, which is also why linear regression underperformed.

Individual genre dummy variables rank lower in this list despite genre clearly mattering
overall. This isn't a contradiction: genre's total effect is spread across ~114
separate one-hot columns, so no single genre column looks important in isolation even
though genre collectively matters a lot.

R² of 0.527 is not a high one, and it likely reflects a genuine ceiling
in the data rather than a modeling failure. Spotify popularity is driven heavily by things
this dataset doesn't capture at all; artist fame, release recency, playlist placement,
marketing. Audio features and genre explain part of the picture.

## Tools

Python, pandas, scikit-learn (LinearRegression, RandomForestRegressor), matplotlib.
