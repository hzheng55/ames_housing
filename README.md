
# Executive Summary

This project was tied to an internal Kaggle competition to predict housing prices for Ames, Iowa, given a dataset from the Ames, Iowa Assessor's Office containing information on houses sold between 2006-2010 [(data dictionary and information can be found here.)](#http://jse.amstat.org/v19n3/decock/DataDocumentation.txt) Our challenge was to build a linear regression model that would most accurately predict housing prices (given 80 housing features to select from and model with.) At the closing of the competition, the 'best' model I built predicted housing prices with an RMSE of 21,167 on the 30% of the data (7th place on the leaderboard), but was drastically overfit on the remaining 70% of the data and came in at 81/90 with an RMSE of 41,187.

### Problem Statement
Using the Ames Housing Data from 2006-2010, build a linear regression model that will most accurately predict housing prices (given 80 housing features to select from and model with.)

### Contents of the Folder:
1. Ames Housing Project Notebook
2. Training Data
3. Test Data
4. Final Kaggle Submission CSV
5. Presentation Slides
------------------
### Modeling Process:
Key decisions I made during the modeling process:

**1. Filled null values with 0:** I didn't drop columns with significant null values. When I examined the columns with nulls, I found most of them weren't really reflecting missing information but were input as 'NA' because a house didn't have that feature (ex. pool). I didn't feel it was necessary to treat categorical and numerical columns differently when filling 'null' cells with 0 because I ultimately dummied out the categorical data anyway.

**2. Dropping Outliers:** the data dictionary provided a clue on outliers in the data, and mapping sale price against above grade living area I was able to identify two that were in our dataset. According to the data dictionary, it's likely the outliers in our data were from unusual partial sales of large properties. I chose to remove the outliers to make the model better predict the majority of housing prices, and this did improve my model's scores.

**3. Mapping Numeric Values to Ordinal Values:** Several of the object-type columns were rating the quality of different features of a house (ex. kitchen, fireplace) with string values instead of numeric values. I thought the model would be more powerful if these were numerical values instead of categorical (dummy) variables, so I created dictionaries of numeric values and mapped those to the columns. I went farther than columns with just the 'Poor' to 'Excellent' ratings and mapped numeric values to any feature that seemed to have a clear 'best' to 'worst' value (ex. Basement Finish). I ended up mapping numeric values to 19 features.

**4. Feature Selection:** for my model, the only features I dropped completely were ID and PID. There were other features that I modified (ex. changed year built to decade and made this categorical) but in general I didn't drop features from the model. I converted categorical variables to dummy variables, used polynomial features on all features, and scaled them.

**5. Normalizing Features:** plotting the distribution of sale prices, there is a strong positive skew that was going to impact the skew of my residuals. I dealt with this by taking the natural log of saleprice (and reversing the log with np.exp after making predictions to convert them back into sale prices at the end.) 

**6. Using Lasso/Elastic Net Regression:** with polynomial features for all features, I needed a strong regularizer to cut down noise and Lasso was more effective than Ridge (best average cross-validated R^2 scores with Lasso were 0.924 on the train data and 0.918 on the test data.) I experimented with Elastic Net (with an l1 ratio of 0.9) on my final model.

### Final Thoughts

This was an interesting challenge and good first foray into linear regression modeling with python, pandas, and sklearn. If I were to try this again, I would have started using GridSearch earlier on to help with model selection and tuning parameters, and spend more time looking at the features and trying to build a simpler model. I also would have spent more time on EDA and data cleaning - there were different decisions I could have made regarding how to deal with missing numerical information (ex. lot frontage) and it would have been interesting to see how that may have made a difference on the model predictions. I would have liked to test my hypothesis on converting ordinal values again against dummying out those variables to see if it did make a difference with my final model.

Finally, being able to translate what my model says about the true predictors of housing price is important and combination features/polynomial features are difficult to interpret, so I could have gone back and tried a simpler model to get clearer coefficients for those individual features (ex. overall quality, total above grade living area).
