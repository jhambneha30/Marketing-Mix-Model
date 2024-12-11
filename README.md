# Marketing-Mix-Model
MMM to establish a relationship between the marketing channels and the dependent variable (sales) so that the budget can be optimized for future marketing channel mix. This can help solve business problems like: What % increase in sales can I expect if I increase my marketing budget by 10% for the next quarter?


# My approach

- Data exploration and data cleaning (data is mostly clean in this case)
- Exploratory Data Analysis - The bar chart shows how revenue is quite different across geographies (giving a clue that some segmentation of data can be done based on geographies). Also, seasonality causes revenue changes.
- Correlation and VIF calculation to eliminate the variables that can cause multicollinearity
- Data pre-processing: Combined the organic and paid YT impressions (this was done as a part of re-iteration after finding that OLS model gives results that indicate high multicollinearity in predictors)
- Data pre-processing: Lag creation and adstocking - assumed the adstock coeff or decay to be 0.3. Note that these generally depend on the business and the product whose info is missing.
- Transformation of predictors in case of diminishing returns - logarithmic or negative exponential (again business dependent)
- OLS (linear regression) model (multiple iterations with & without lags, different values of adstock etc.). Results showed that there is possibility of multicollinearity in predictors and p-values show that the predictors are not statistically significant. Affiliate Impressions coefficient came out to be negative (hinting at the possibility that this type of marketing might not be generating revenue), but the coefficients are not reliable due to multicollinearity hence can’t be sure.
- To tackle multicollinearity, there are approaches like combination of predictors. Running secondary residual models and regularization (using ridge, lasso or elastic net). I chose regularization with Ridge regression as I wanted to retain all predictors. (Lasso could be used in case of high dimensionality which is generally not the case in MMM.)
- After tuning alpha, the results made some sense, R squared came out to be ~0.7. 
- To calculate ROI, the assumption taken is that the column Google Spend contains the combined spend for YT Paid Impressions, Google Impressions and Email.

# Which marketing channels have max and min impact on the revenue?
- **FB_IMP and GOOGLE_IMP** have the largest positive coefficients, indicating that these channels have the highest marginal contribution to revenue.
- **EMAIL_IMP** is also highly impactful, demonstrating strong efficiency.
AFF_IMP and YT_PAID_IMP contribute the least, indicating lower efficiency or importance relative to other channels.
- Confidence on the interpretation:
- R squared ~ 0.7 which makes it a moderately good fit.
- Ridge regularization helps in stabilizing the otherwise unstable OLS model, as it reduces variance by generalizing the model and tackles multicollinearity
Note that the coefficients from Ridge are directional indicators of the performance of marketing channels. If the spend data was provided for Email and YT paid impressions, the confidence on results would have been higher.

NOTE: Interaction effects generally exist between the media channels. This approach again requires some business context. Another approach is transferring the impact from one channel to another as a pathway.


# Is the budget being spent effectively across channels? If not, how would you reallocate it?
No, the business is not allocating spend across channels efficiently as of now.
- Channels like FB, GOOGLE, and EMAIL seem to provide higher returns, suggesting the budget should focus on these.
- However, the budget for Affiliate marketing and Youtube Impressions need re-consideration.
- The best way to decide the re-allocation strategy is through budget optimization (using optimization algorithms like COBYLA or SLSQP. We can run an optimizer by constraining the overall budget to see the best mix of marketing budget. The objective can be to maximize revenue or maximize profit (business dependent)
- Another way is simulating budget shifts to understand potential gains or losses from reallocating funds between channels.


# Do channel performances vary by geography? Should the model account for these differences?
There are differences across geographiess:
- Initial EDA (bar chart) shows the difference in revenue across geographies, indicating that the geographies need to be grouped & these groups can then be modeled separately.
- The Mixed effect model results indicate high variance values suggest significant differences in how predictors affect REVENUE across geographies. 
I would incorporate geo-level differences using one of the following approaches.
* Using a Mixed-Effects Model is highly recommended:
* Include geo-specific random effects to capture variations across geographies.
* Retain channel coefficients as fixed effects, ensuring interpretability at the global level while allowing for localized adjustments.
* Group/Cluster the geographies (using some external info, K-means can be used for clustering). Generate a separate model for each group and then optimize the budget.
Note that we can also model every geography separately

# What other insights or recommendations would you share with the team?
* **Observation (seasonality/trend):** 
The revenue shoots up during the holiday season i.e. around Christmas and New years. When checked at geography level, it seems that the sales increase for all geographies during this season but by different amounts. Seasonality can be modeled as a control variable for better predictions.
* **Recommendation:**
Evaluate the saturation and diminishing returns for each channel using adstock curves. For example, channels like FB and GOOGLE might have diminishing returns despite their high coefficients.
* **Recommendation:**
Provide spend for each of the channels to get insights based on spend-based ROI analysis rather than impression-based metrics to make results actionable for budget allocation.




