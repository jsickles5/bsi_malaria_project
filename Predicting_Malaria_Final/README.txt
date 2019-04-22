Malaria Outbreak Prediction in Nigeria

Contributors: Brian Liggio, Jessica Sickles, Prashant Toteja, Keely Meyers

Background:

We are a group of students from the University of Michigan School of Information studying Information Analysis.  We are working with Abt Associates, a global healthcare consulting firm, on a capstone project to predict malaria outbreak in Nigeria.

Project Goals and Description:

The overarching goal for this project was to create a model that would predict malaria outbreaks in Nigeria. The data for the first approach we took consists of various spatial images from open data sources (Malaria Atlas Project and Terraclip). These images were manually cropped, scaled, and transformed in order to be compatible with our machine learning models. The data analysis portion of the project included preliminary data cleaning, transforming/vectorizing image data to a Pandas dataframe, and creating a basic prediction visualization based on various machine learning techniques. These steps produced initial static images based on the collected spatial data.  For the second approach, we collected survey and API data.  The survey data came from the Malaria Atlas project, and the API data came from the Google Maps, GeoNames, and DarkSky APIs.  We created an OLS model and a Random forest model with our new variables.  We used the Folium package in Python to create an interactive visualization of our new predictions.

Approach 1: Spatial Imagery

	Data Collection: Our first approach was based on using geospatial images as our data. We found open source spatial imagery from the Malaria Atlas Project and IPUMS.  We chose 14 static images from these sources to be used in our analysis. We chose to use "endemicity" as our Y variable. In our regression model, we define endemicity as the predicted percentage of malaria prevalence in a given area.

		Predictor Variables:
		resistance: Mean estimates of sickle cell haemoglobin allele frequency

		fever_proportion: Malaria-positive fevers as a proportion of all-cause fevers in 2015 in Nigeria

		mosquito_proportion: The spatial distribution of Plasmodium falciparum malaria endemicity in 2015 in Nigeria

		temp_suitability: The annual temperature suitability index for Plasmodium falciparum transmission which reflects the impact of daily temperature on vector survival

		proximity_to_urban: Modeled accessibility to high-density urban centers for 2015 in Nigeria (measured by travel time)

		urban: Modeled distribution of urban centers

		annual_mean_temp: Annual mean temperature

		cropland: Distribution of natural vegetation

		precipitation: Precipitation seasonality

		vegetation: Areas of primary vegetation

		annual_temp_range: Annual temperature range

		water_bodies: Locations of all water bodies

	Data Processing: The first challenge with data processing was that many of the images were different sizes and different heat scales.  We used Photoshop to tackle this problem.  The first step was to convert each image to greyscale. Then we cropped each image and converted the cropped images into smart objects.  We chose 386*312 as standardized pixel size that we would scale each image to.  We used layers in Photoshop to allow us to resize each image to fit the standard dimensions.  These images were then saved as .png files and loaded into a our Python script as Pandas dataframes.

	Analysis: We built three different models to perform analyses on our data

		Linear regression: We used an Ordinary Least Squares regression in order to predict areas of high or low risk of malaria.  We used endemicity as our y variable, and the predictor variables mentioned above as our x variables.  The model achieved an R-squared value of over 0.93 and an accuracy score over 0.9.  The predictions were then mapped from an array back to an image to produce a static output. The static output resembles a map of Nigeria with areas of higher risk colored darker than areas of lower risk.

		Random Forest: We used the Sklearn Python module to run a random forest regressor with 1000 trees.  We achieved a fit score of 0.98.  We observed the feature importance determined by the model, and created an output table based on this metric.  This dataframe was used to make predictions about areas of high and low risk for malaria in Nigeria, and display those predictions on another static map.  

		Neural Network:  We used the TensorFlow Keras open source machine learning library to create a model that we believe would more accurately represent the spread of epidemic in the real world.  This model was intended to use the areas around a point of interest to help predict the malaria risk factor of that specified point.  The model was created with two hidden layers with 12 and 8 nodes respectively.  This model generated a mean absolute error of 0.65.

	Mapping: Creating visualizations for these models was difficult because of the type of data we were using.  Our spatial data lacked geo-coordinates, which made it significantly more difficult to map our prediction to point on our map.  We were able to create a static image based on our OLS model to visualize our predictions.  However, our map lacked interactivity, and the heatmap element lacks color and proper contrast.

Approach 2: Survey Data Prediction

	Data Collection: Our second approach was based on survey data and data generated from different geography based APIs. We found open source survey data from the Malaria Atlas Project on malaria outbreaks tied to geocoordinates.  We also made calls to the Google Maps, GeoNames, & DarkSky APIs to gather data for our predictor variables. Additionally, we found an open source spreadsheet of healthcare center locations in Nigeria. We filtered these to only include those that held and administered malaria treatment.  

		Predictor Variables:
		population: population of location/ nearest city for year being modeled.

		climate: calculating average temperatures (observed and perceived), dew point, humidity, wind speed and direction (all different variables)

		distance_to_closest_care_center: calculated distance to closest healthcare center 

		rural: binary variable indicating whether location is rural or not (1) or not (0)

		urban: binary variable indicating whether location is urban or not (0) or not (1)

		A (Geofeature): binary variable indicating state/country 

		L (Geofeature): binary variable indicating parks/agricultural area

		P (Geofeature): binary variable indicating the presence of a city or village

		R (Geofeature): binary variable indicating the presence of roads/railroads

		T (Geofeature): binary variable indicating mountain/hill/natural feature

	Data Processing: We cleaned up the data by loading each of the three source into our Python scripts.  We converted each to a dataframe in Pandas and combined all of them into a single dataframe.  We then removed some null values from our dataframe and changed some column names to produce a final workable dataframe.

	Analysis: We built two different models to perform analyses on our data with this approach.

		Linear Regression: We created a Ordinary Least Squares regression, similar to Approach One.  We used this model to attempt to identify the features with the greatest importance. We found urban/rural location, humidity, and parks/land cover to be most highly correlated with outbreaks, which makes sense given areas with higher populations (urban centers) probably face higher severity when it comes to epidemics. Additionally, we believe humidity was significant because there are ideal climates for the disease-carrying mosquitoes to thrive.


		Random Forest: We ran a similar Random Forest regressor to the one in Approach One. The model achieved a fit score of 0.8, accuracy score of 0.93, and a mean absolute error of 0.25.  In this model, the most important features were determined to be distance to closest care center, temperature, and wind speed. Similarly, the latter two attributes probably have to do with mosquito habitats. Meanwhile, distance to the nearest care center probably has to do with the severity of the outbreaks.

	Mapping: We used the Folium package in Python to create an interactive visualization for this approach. This allows for users to zoom in/out and click on markers that are deemed as areas of high risk and see descriptive data about that place and its exact geo-coordinates. This can be used by service workers in Nigeria to conduct sprayings and distribute bed nets in areas identified as high risk before any outbreak can occur. 




