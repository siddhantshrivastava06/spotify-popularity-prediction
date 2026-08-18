# Spotify Song Popularity Prediction

Predicting a song's Spotify popularity score (0-100) from its audio features and genre.

## Dataset

114,000 tracks from Spotify, 21 columns including audio features (danceability, energy,
loudness, acousticness, valence, tempo, etc.), genre, and a popularity score.
Source: "Spotify Tracks Dataset" (Kaggle - maharshipandya).

## Question

Can audio characteristics of a track predict how popular it is?

## My Approach

1. **EDA (Exploratory Data Analysis)** - analyze data (drop missing values etc.),
2. plotted the popularity distribution, and computed correlations between each audio
   feature and popularity.
3. **Key finding from EDA** - every individual audio feature has near-zero linear
   correlation with popularity (max |r| ≈ 0.10). Genre, however, has a large effect:
   average popularity ranges from 59 (pop-film) down to 2 across genres.
   This suggested the signal, if it existed, wasn't linear or wasn't in single features
   alone.
4. **Baseline model** - linear regression on audio features.
5. **Improved model** - random forest, to test whether the signal was there but non-linear
   (interactions between features) rather than absent.

## Results

| Model             | RMSE (Root Mean Squared Error)  | MAE (Mean Absolute Error)  | R²    |
|-------------------|-------|-------|-------|
| Linear Regression | 19.12 | 14.09 | 0.259 |
| Random Forest     | 15.28 | 10.32 | 0.527 |

Random forest roughly doubled R² over the linear baseline, confirming the EDA hypothesis:
the signal was there, just non-linear. Linear regression can't model feature interactions;
random forest can.

## Conclusion

No single feature dominated, importance spread evenly across ~10 audio features, which is why random forest beat linear regression. It's weak signals combining, not one strong lever. Genre's individual columns ranked lower too, but that's because its effect is spread across ~114 one-hot columns, not because genre stopped mattering. R² of 0.527 reflects a real ceiling, Spotify popularity depends heavily on things this dataset doesn't have at all (artist fame, playlist placement, marketing).

## Tools

Python, pandas, scikit-learn (LinearRegression, RandomForestRegressor), matplotlib.
