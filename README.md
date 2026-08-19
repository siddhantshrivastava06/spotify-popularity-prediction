# Spotify Song Popularity Prediction

Predicting a song's Spotify popularity score (0-100) from its audio features and genre.

## Dataset

114k tracks from Spotify, 21 columns including audio features (danceability, energy,
loudness, tempo, etc.), genre, and a popularity score.

From: "Spotify Tracks Dataset" (Kaggle - maharshipandya).

## My Question

Can audio characteristics of a song predict how popular it is?

## Results

| Model             | RMSE (Root Mean Squared Error)  | MAE (Mean Absolute Error)  | R²    |
|-------------------|-------|-------|-------|
| Linear Regression | 19.12 | 14.09 | 0.259 |
| Random Forest     | 15.28 | 10.32 | 0.527 |

Random forest roughly doubled R² over linear regression. So there indeed is a correlation, just not a 
linear one. 

## Conclusion

No single feature dominated, importance spread evenly across ~10 audio features, which is why random forest beat linear regression. It's weak signals combining, not one strong lever. Genre's individual columns ranked lower too, but that's because its effect is spread across ~114 one-hot columns, not because genre stopped mattering. R² of 0.527 reflects a real ceiling, Spotify popularity depends heavily on things this dataset doesn't have at all (artist fame, playlist placement, marketing).

## Tools

Python, pandas, scikit-learn (LinearRegression, RandomForestRegressor), matplotlib.
