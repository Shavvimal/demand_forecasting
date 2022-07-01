# Demand Forecasting Project 

## File Tree & Descriptions
 * **Metric_Comparison**: 
```These are a collection of notebooks intended for the comparison of the performance of Various demand forecasting models. Various metrics are used such as MAE, WMAPE, Aggregated WMAPE, wQL[0.1], wQL[0.5] (WMAPE), wQL[0.75], wQL[0.9], Mean wQL```
     + [compare_forecasts.ipynb](https://github.com/Shavvimal/demand_forecasting/blob/main/Metric_Comparison/compare_forecasts.ipynb)
        + Joes inital Comparison using squared error. He compares an old TFT output to LSTM, ARIMA and Naive predictions.
        + He also plots the worst performers from each models and identidfies scenarios where the models do not perform as well
     + [metric_comparison_SV.ipynb](https://github.com/Shavvimal/demand_forecasting/blob/main/Metric_Comparison/metric_comparison_SV.ipynb)
        + Notebook contains comparisons between initial models using various metrics (MAE	WMAPE	Aggregated WMAPE)
        + Also contains code at bottom showing timeseries where the TFT performs badly. This function picks out WMAPE of models higher or lower than a certain threshold
            + The conclusion was the TFT does badly when the sales go to 0. This was an issue we were facing; naive will outperform the TFT if sales go to 0, as it will just also predict 0. We need to somehow inout phase in phase out.
        + Shows aggregated WMAPE for 6 months into the future
     + [metric_comparison_with_wQL_and_more_models.ipynb](https://github.com/Shavvimal/demand_forecasting/blob/main/Metric_Comparison/metric_comparison_with_wQL_and_more_models.ipynb)
        + In order to compare model perofrmance we need an estimate of bayes limit for the problem. In this case we do not have a human error reference. So we use the the lowest of the following: APO forecast, Persistence 1 month, Persistence 3 month
        + Contains background on Weighted Quantile Loss. This loss is the one we had identified from our reading that would be ideal. 
            + Amazon Forecast also calculates the average wQL, which is the mean value of weighted quantile losses over all specified quantiles. By default, this will be the average of wQL[0.10], wQL[0.50], and wQL[0.90].
        + All togheter, we compare using: MAE, WMAPE, Aggregated WMAPE, wQL[0.1], wQL[0.5] (WMAPE), wQL[0.75], wQL[0.9], Mean wQL
        + As an example, we have imported the npy predictions for actuals_output, naive_predictions, TFT_predictions, ARIMA, dual_TFT, LSTM, multi_TFT_v3, TFT_encoded, TFT_predictions_v2
 * **Models**: ```This Folder collates the models which we have tried for Demand forecasting.``` 
     + [ARIMA_model.ipynb](https://github.com/Shavvimal/demand_forecasting/blob/main/Models/ARIMA_model.ipynb)
        + Basic ARIMA model and export of predictions
     + [Copy_of_Multi_Variate_Time_Series_Forecasting.ipynb](https://github.com/Shavvimal/demand_forecasting/blob/main/Models/Copy_of_Multi_Variate_Time_Series_Forecasting.ipynb)
        + This notebook formulates a multi-variable forecasting problem to predict the next 9 months of Volume. Here I compare the forecasting performance of using several different model types. Each model uses the same final two DNN layers with dropout. One of 128 units, and the final layer of 24 (the output horizon). Each of the models unique layers are: 
            + A three layer DNN (one layer plus the common bottom two layers)
            + A CNN with two layers of 1D convolutions with max pooling.
            + A LSTM with two LSTM layers.
            + A CNN stacked LSTM with layers from models 2 and 3 feeding into the common DNN layer.
            + A CNN stacked LSTM with a skip connection to the common DNN layer.
        + Each model is compared against baseline persistance models consisting of a one month persistence, and a three month average persistence. Also compared is the TFT error.
        + In summary, these models arent good, but are good for compariosn. 
     + [dilatedcnn_multi_round.ipynb.ipynb](https://github.com/Shavvimal/demand_forecasting/blob/main/Models/dilatedcnn_multi_round.ipynb.ipynb)
        + Not my code, but a Dilated CNN could also be something to investigate for whoever takes over this project. 
     + [forecasting_TFT.ipynb](https://github.com/Shavvimal/demand_forecasting/blob/main/Models/forecasting_TFT.ipynb)
        + This is, what i think, the most up to date TFT. This code is heavily taken from the [documentation](https://pytorch-forecasting.readthedocs.io/en/stable/tutorials/stallion.html) which i would highly reccomend reading and playing around with before. 
     + [LSTM_forecast_model.ipynb](https://github.com/Shavvimal/demand_forecasting/blob/main/Models/LSTM_forecast_model.ipynb)
        + An LSTM used for demand forecasting. 
 * **Preprocessing**: ``` In general, these are portions of code to help with preprocessing of data```
     + [get_forecast_data.ipynb](https://github.com/Shavvimal/demand_forecasting/blob/main/Preprocessing/get_forecast_data.ipynb)
        + Converting the data format found in the SQL database into something that can be fed into the TFT. This includes things like:
            + Keep only actual sales and remove uneeded cols
            + Renaming colums 
            + Making an artificial time index to pass into dataframe builer
            + Create average cols for groups, remove volume under zero
            + Only use id we want to , and removing the final time index
        + This has all been funcitonalised.  
     + [look_at_forecasts.ipynb](https://github.com/Shavvimal/demand_forecasting/blob/main/Preprocessing/look_at_forecasts.ipynb)
        + Contains code from above, but also looks at the forecasts we have been given from SQL
        + Look into forecasts e.g.: Determine how many forecasts are zero when the last few months are zero
     + [preprocess_short.ipynb](https://github.com/Shavvimal/demand_forecasting/blob/main/Preprocessing/preprocess_short.ipynb)
        + This is the same as get_forecast_data.ipynb but done step by step.
     + [preprocessing_JA_v2.ipynb](https://github.com/Shavvimal/demand_forecasting/blob/main/Preprocessing/preprocessing_JA_v2.ipynb)
        + Main preprocessing turning extra vars and raw dataframe into dataframe to be put into TFT
        + Creation of variables such as: 
            + log_volume
            + avg_volume_by_plant
            + max_volume_by_plant
            + min_volume_by_plant
            + std_volume_by_plant
            + std_volume_by_plant
            + avg_volume_by_material
            + max_volume_by_material
            + min_volume_by_material
            + std_volume_by_material
            + std_volume_by_material
     + [timeseries_AE.ipynb](https://github.com/Shavvimal/demand_forecasting/blob/main/Preprocessing/timeseries_AE.ipynb)
        + Coding an Autoencoder to try to compress extracted temporal features into encodings to be used as static variables in TFT
     + [timeseries_analytics.ipynb](https://github.com/Shavvimal/demand_forecasting/blob/main/Preprocessing/timeseries_analytics.ipynb)
        + Timeseries visualisation
        + Average volume over months
        + Plot for total volume in each plant
        + Perform Dicky Fuller test on total volume over each month to see if it is stationary
            + Null hypothesis is that data is non stationary. If P-value is less than 0.05 then we can reject the null hypothesis (Data is stationary). If P-value is more than 0.05 then we cannot reject the null hypothesis (Data is non-stationary).
        + Decompose timeseries into trend, season and residuals
        + Use transform data to make stationary
        + Autocorrelation
            + Autocorrelation is a simple method to determine how much one timestep is related to future timesteps. This is acheived by shifting a timeseries one into the future iteratively and finding the correlation coefficient between the origonal timeseries and the shifted timeseries. This is essentially the gradient of the best fit line through the two sets of timeseries
     + [ts_classifier.ipynb](https://github.com/Shavvimal/demand_forecasting/blob/main/Preprocessing/ts_classifier.ipynb)
        + Coding a classifier to try to predict the number of zeros in the 6 month prediction period of a timeseries
 * **server_connection**: ``` Functions for connection to a Server. This code has been inclided from the bladder leak project, but editing the code is simple```
     + [sql_query.py](https://github.com/Shavvimal/demand_forecasting/blob/main/server_connection/sql_query.py)
     + [turbo_connection.py](https://github.com/Shavvimal/demand_forecasting/blob/main/server_connection/turbo_connection.py)
