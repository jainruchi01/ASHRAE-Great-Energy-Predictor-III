Summary of Pre-processing and Modeling:
----------------------------------------
1. Combined all the train data frames into one.
2. Removed outliers: data points for building id 1099 and 778
3. Removed the data points which had zero meter readings
4. Added new features from timestamp column: Month, Weekday, Hour
5. Missing value Imputation: 
	a. year_built: Imputed with mean/meadian based on site_id
	b. floor_count: Imputed with mean/meadian based on site_id
		There were some sites for which floor_count value was still na, so imputed them with one (min floors a building can have)
	c. Imputed below features with mean/meadian based on 'site_id', 'weekday', 'month' 
		and for some features there were still some na values so imputed them with overall mean/meadian
		air_temperature,cloud_coverage,dew_temperature,precip_depth_1_hr,sea_level_pressure,wind_direction,wind_speed
6. Added is_holiday feature based on USFederalHolidayCalendar
7. Added season feature based on US seasons
8. Added Is Day feature: if its day time hours or not
9. Added relative humidity feature
10: Normalization: used log1P normalization on floor_count, square_feet and meter_reading
11. Time based splitting of train data into training (80%) and cross validation (20%)
12. Label Encodeing: primary_use, season
13. Based on VIF factor for multicollinearity (http://statisticshowto.com/variance-inflation-factor/)
	dropped below features:
	'building_id', 'air_temperature','sea_level_pressure'
14. Final feature set on which the models are trained:
	['meter','site_id','primary_use','square_feet','year_built','floor_count','cloud_coverage','dew_temperature',
	'precip_depth_1_hr','wind_direction','wind_speed','month','weekday','hour','is_holiday','season','IsDay','relative_humidity'] 
15. Below models are used for prediction on test data:
	a. LGBM Regressor with GBDT
	b. Decision Tree Regressor with depth of 15
	c. CatBoostRegressor with number of estimatores as 1000