# Barcelona Traffic Accident Classification Project

## What This Project Does
This project tries to predict why traffic accidents happen in Barcelona using machine learning. By looking at accident data from the Guardia Urbana, we can spot patterns and predict likely causes based on things like where and when accidents happen. City planners and police can use this info to make roads safer.

## About the Data
The dataset has details about Barcelona traffic accidents including:

### Main Information Collected
- **Numero_expedient**: ID number for each accident
- **Nom_districte**: Name of the district
- **Nom_barri**: Name of the neighborhood
- **Descripcio_dia_setmana**: Day of the week (like "Divendres" for Friday)
- **Hora_dia**: Time of day
- **Descripcio_torn**: Time of shift (Morning, Afternoon, Night)
- **Descripcio_causa_mediata**: Main cause of the accident (this is what we're trying to predict)
- **Coordenada_UTM_X_ED50 / Coordenada_UTM_Y_ED50**: Location coordinates

## Data Cleanup
I did a few things to get the data ready:
- Removed duplicate records
- Filled in missing information
- Fixed date formats
- Created new useful features from existing data

## What I Found During Data Exploration

### Causes of Accidents
There's a big imbalance in the causes:
- The most common cause was class 9 (with 2061 instances)
- Some causes are very rare, like class 7 (only 15 instances) and class 6 (25 instances)

### Where Accidents Happen
I looked at how accidents are spread across different neighborhoods and districts.

### When Accidents Happen
I found patterns in what days and times accidents are more likely to happen.

## New Features I Created

### Time-Related Features
- Used special math (sine and cosine) to represent the time of day better for the computer
- Added a weekend indicator
- Created a barrio-turno interaction feature that combines neighborhood with time of day

### Location Features
- Calculated distance to the city center
- Created neighborhood groups based on similar accident patterns

## How I Built the Models

### Models Tried
I tested two main models:
- LightGBM 
- CatBoost 

### Dealing with Unbalanced Data
Since some accident causes are much more common than others, used:
- SMOTE-Tomek to create balanced training data
- Added class weights to give extra importance to rare causes

### Finding the Best Settings
- Used Optuna to automatically find the best model settings
- Ran 20 trials to find optimal parameters
- Used cross-validation to make sure our results are reliable

## How Well the Models Performed

### LightGBM Results
- **ROC AUC**: 0.5560
- The model did best on class 9 (precision: 0.26, recall: 0.37)
- Also did well on class 14 (precision: 0.19, recall: 0.17)
- Struggled with rare classes (6, 7, 12)

### CatBoost Results
- **ROC AUC**: 0.5462
- Had different strengths, including better performance on class 12 (recall: 0.29)
- Overall slightly lower performance than LightGBM

### Most Important Features
For LightGBM, the top features were:
1. Coordinates (UTM_X and UTM_Y)
2. Distance to city center
3. Neighborhood-shift combination
4. Month of the year

For CatBoost, the top features were:
1. Month of the year
2. Neighborhood
3. Coordinates (UTM_X and UTM_Y)
4. Distance to city center

## Challenges Faced
- Big imbalance in the causes made rare accidents hard to predict
- Barcelona's complex layout creates location-specific patterns
- Seasonal factors affect accident patterns

## What I Learned
- Where and when accidents happen strongly influences why they happen
- Location and time are the most important factors for prediction
- I can use this to suggest specific safety measures for high-risk areas

## Future Improvements
- Try a two-step prediction approach (first general cause category, then specific cause)
- Create more advanced features from the location and time data
- Combine multiple models for better performance
- Add weather and traffic volume data
- Try deep learning approaches