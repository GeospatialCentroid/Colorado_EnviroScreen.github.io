This tutorial has 3 major sections
- [Setting up the local environment](https://geospatialcentroid.github.io/COEnviroScreen/dataProcessing/localEnv.html)
- [running the processing code](https://geospatialcentroid.github.io/COEnviroScreen/dataProcessing/processData.html)
- [details on specific functions](https://geospatialcentroid.github.io/COEnviroScreen/dataProcessing/individualFunctions.html)


## Individual Function Details

The are three general classes of functions within the code base; processing, compiling, and helper.

- **Processing functions** are performing some type of analysis and returning new dataset.
  - ex: "asthma.R"
  - users are expected to adjust the input parameters which forcing specific rewrites or adjusting the input file path
- **compiling functions** calling processing functions or pulling data from existing source and combining datasets.
  - ex: "environmentalExposures.R"
  - users are expected to adjust input parameters when forcing specific rewrites or changing version of data generated.
- **helper functions** are chunks of processing code that are used by multiple processing functions. They are stored in "r/utils" and generally have set input parameters.
  - ex: bufferObjects
  - users are not expected to directly edit input parameters.


A short description of each function will be provided below.

Index for function descriptions

**processing functions**
- acs.r
- asthma.r
- drinkingWater.r
- drought.r
- ej_screen.r
- floodplain.r
- getCoal.r
- getDI.r
- getJustice40.r
- getOilGas.r
- getRural.r
- haps.r
- heartDisease.r
- heatDays.r
- houseBurden.r
- lifeExpectency.r
- lowBirthWeight.r
- mining.r
- noise.r
- otherHaps.r
- ozone.r
- placesData.r
- pm25.r
- proxyOilGas.r
- surfaceWater.r
- wildfire.r


**compiling functions**
- climate.r
- environmentalExposures.r
- environmentalEffects.r
- finalComponentScore.r
- getShinyData.r
- joinDataFrames.r
- processData.r
- sensitivePopulations.r
- socioEconomic.r



**helper functions**
- createFolderStructure.r
- getGeometryLayers.r
- gm_mean.r
- loadFunctions.r
- normalizeVector.r
- patternLayer.r
- setSpatialData.r


**Template**
**Name**: file name

**Function Name**: how to call the function in R

**Description**: What does the function do

**Inputs**: list of input parameters

**Outputs**: description of output values

**helper function**: list of helper functions used

**Updating Data**: What to consider when updating the input dataset

**Other Considerations**: Other notes and thoughts about the process



### Processing Functions


#### standard inputs
Not all functions have all these inputs but most do.

**filePath**: Location of the file within the input folder. For functions in which there are more then one input features, the filepaths are defined directly within the function.

**geometry**: Spatial object representing the geography of interest. Generally has some role in the processing of the dataset.

**processingLevel**: character vector describing the geography of interest ("county", "censusTract", "censusBlockGroup"). Generally only used to constructing file paths.

**version**: character value used to represent the version of the enviroscreen dataset. A new version will force all data processings scripts to regenerate content.

**overwrite**: Binary TRUE/FALSE to define if a existing file should be overwriten. IF FALSE the existing file will be read in and returned as a dataframe.

---

**Name**: acs.r

**Function Name**: `asc()`

**Description**: Pulls 2019 ACS data utilizing the `tidycensus` library and calculated specific measures.

**Inputs**:
- processingLevel
- version
- overwrite

**Outputs**:
- dataframe with GEOID, eight indicator values, and a total population value used in generation of data for shiny applications.
-  saves dataframe as csv to

**helper function**: NA

**Updating Data**: 2019 ACS data is not expected to change. An update to 2020 census data is dependent on availability of health metrics.

**Other Considerations**: requires a census api key.

---

**Name**: asthma.r

**Function Name**: `getAsthma()`

**Description**: Reads in census tract level asthma hospitalization rate data and generalizes to specific geography.

**Inputs**:
- filePath
- geometry
- processingLevel
- version
- overwrite

**Outputs**:
- dataframe with GEOID and adjusted asthama hospitalization rate
-  saves dataframe as csv to

**helper function**: NA

**Updating Data**: ensure column headers remain consistent between new and old version of the dataset.

**Other Considerations**: NA

---

**Name**: drinkingWater.r

**Function Name**: `getDrinkingWater()`

**Description**:Multiple step evaluation relating number and longevity of drinking water violations to effected populations at the county level.

**Inputs**:
- geometry
- processingLevel
- version
- overwrite

**Outputs**:
- dataframe with GEOID and adjusted asthama hospitalization rate
-  saves dataframe as csv to

**helper function**: NA

**Updating Data**: Three input datasets, some sensitivity to column names and `read.csv()` calls.

**Other Considerations**: Multiple input datasets are defined within the codebase. Fairly complex processing effort due to the number of steps.

---

**Name**: drought.r

**Function Name**: `getDrought()`

**Description**: Summarize an area adjusted drought score for all counties and applies it to smaller geographies.

**Inputs**:
- filePath
- geometry
- processingLevel
- version
- overwrite

**Outputs**:
- dataframe with GEOID and drought measures
-  saves dataframe as csv to

**helper function**: NA

**Updating Data**: Easy to update do to standard data source. There are specific filter calls to look at the 2016:2020 years in the code. Those would need to be changes if input data was from a new time period.

**Other Considerations**: NA

---

**Name**: ej_screen.r

**Function Name**: `ej_screen()`

**Description**: Pulls specific data from the larger EJScreen dataset and generalizes to requested geography.

**Inputs**:
- filePath
- geometry
- processingLevel
- version
- overwrite

**Outputs**:
- dataframe with GEOID and drought measures
-  saves dataframe as csv to

**helper function**: NA

**Updating Data**: Easy to update do to standard data source.

**Other Considerations**: NA

---

**Name**: floodplain.r

**Function Name**: `getFloodplain()`

**Description**: Calculated the total area of a geography within an active floodplain.

**Inputs**:
- filePath
- geometry
- processingLevel
- version
- overwrite

**Outputs**:
- dataframe with GEOID and drought measures
- saves dataframe as csv

**helper function**: NA

**Updating Data**: Unlikely to be able to use the same source for future updates. The processing code is not complicated but don't expect to be able utilize it as is when new floodplain feature is found.

**Other Considerations**: The floodplain input dataset is a complex polygon and therefore all the intersections require to generate results tend to be slow. This is not a script you want to rerun unless you have to.

---

- getCoal.r

**Name**: getCoal.r

**Function Name**: `getCoal()`

**Description**: Generates a RDS file for visualization and defining of coal communities in Colorado.

**Inputs**: NA

**Outputs**:
- RDS file for visualization in shiny application
- RDS file used for compiling the dataset used in the shiny application

**helper function**: patternLayer

**Updating Data**: Coal counties are define by a list within the funciton. Editing this list will result in new content.

**Other Considerations**: The floodplain input dataset is a complex polygon and therefore all the intersections require to generate results tend to be slow. This is not a script you want to rerun unless you have to.


---

**Name**: getDI.r

**Function Name**: `getDI()`

**Description**: Utilizes `tidycensus` to grab acs data and define disproportionality impacted communities in Colorado.

**Inputs**:
- removeNativeLands
- overwrite

**Outputs**:
- RDS file used for visualization and compiling the dataset used in the shiny application

**helper function**: NA

**Updating Data**: Since the DI communities are not actually a part of the enviroscreen score calculation, this could be one of the first map features to utilize the 2020 census changes in geographies. That said, it is tied to a house bill so there might be some stipulation in changing this.

**Other Considerations**: removeNativeLands is a binary that excludes specific census tracts and census blocks that are part of the native american reservations in Colorado. This may need to be adjusted as work communication continues with those entities. overwrite is used in this case because there is a direct call on ACS data. It's recommened that overwrite=false is the standard to avoid numerous api calls.

---

**Name**: getJustice40.r

**Function Name**: `getJustice40()`

**Description**: Summarizes an existing dataset for visualization within the shiny applications.

**Inputs**:
- filepath
- removeNativeLands
- overwrite

**Outputs**:
- RDS file used for visualization and compiling the dataset used in the shiny application

**helper function**: NA

**Updating Data**: Standard input dataset. Should be easy to update.

**Other Considerations**: removeNativeLands is a binary that excludes specific census tracts and census blocks that are part of the native american reservations in Colorado. This may need to be adjusted as work communication continues with those entities. overwrite is used in this case because this process requires reading in some large files and therefore takes some time.


---

**Name**: getOilGas.r

**Function Name**: `getOilGas()`

**Description**: Generate a spatial object of all the counties in Colorado with Oil or Gas development.

**Inputs**: NA

**helper function**: patternLayer

**Outputs**:
- RDS file used for visualization in the shiny apps.
- RDS file used for data within the shiny apps.

**Updating Data**: Requires adjusting the County names in vector within the function.

**Other Considerations**: NA




---

**Name**: getRural.r

**Function Name**: `getRural()`

**Description**: Generate a spatial object of all the rural counties in Colorado.

**Inputs**: NA

**helper function**: patternLayer

**Outputs**:
- RDS file used for visualization in the shiny apps.
- RDS file used for data within the shiny apps.

**Updating Data**: Requires adjusting the County names in vector within the function.

**Other Considerations**: NA

---

**Name**: haps.r

**Function Name**: `getHAPS()`

**Description**: Applies an relative ranking and distance weight method to individual point source pollution measures.

**Inputs**:
- filePath
- geometry
- processingLevel
- version
- overwrite


**helper function**: bufferObjects, normalizeVector

**Outputs**:
- dataframe with GEOID and singular HAPS score meaure
- saves dataframe as csv

**Updating Data**: Comes from APENS which is a standardized data source so updating should be easy. Temporal filtering is done in the generation of the input dataset.

**Other Considerations**: This is one of the more complicated processing methods. Three levels of evaluation. Normalization among specific indicators, distance weighted buffering method, and aggregation across all individual indicators.

---

**Name**: heartDisease.r

**Function Name**: `getHeartDisease()`

**Description**: Aggregates census tract level heart disease data to census block groups. County and census tract values are provided in the datasource.

**Inputs**:
- filePath
- geometry
- processingLevel
- version
- overwrite


**helper function**: NA

**Outputs**:
- dataframe with GEOID and heart disease score measure
- saves dataframe as csv

**Updating Data**: There is some specific text formating done to work with the column headers of the input datset, be sure to watch if alterations still apply to updated versions of the data.

**Other Considerations**: NA


---

**Name**: heatDays.r

**Function Name**: `getHeatDays()`

**Description**: Calculates the average number of heat days at the census tract level for 2015-2019 and aggregates those values to the county and census block group levels.

**Inputs**:
- filePath
- geometry
- processingLevel
- version
- overwrite


**helper function**: NA

**Outputs**:
- dataframe with GEOID and average number of extreme heat days over a 5 year period
- saves dataframe as csv

**Updating Data**: Temporal filtering is applied  to the input dataset, update the comments in the code once the year range changes.

**Other Considerations**: NA


---

**Name**: houseBurden.r

**Function Name**: `getHousingBurden()`

**Description**: Calculates the percentage of households that are housing burdened within a given area.

**Inputs**:
- geometry
- processingLevel
- version
- overwrite


**helper function**: NA

**Outputs**:
- dataframe with GEOID and average number of extreme heat days over a 5 year period
- saves dataframe as csv

**Updating Data**: Input data is called directly from the 2019 ACS via tidy census. Can not update to 2020 until accounting for the redistricting of the decal census.

**Other Considerations**: NA


---

**Name**: lifeExpectency.r

**Function Name**: `getLifeExpectancy()`

**Description**: Aggregates life expectancy measured at the census tract level to county and census block groups.

**Inputs**:
- filePath
- geometry
- processingLevel
- version
- overwrite


**helper function**: NA

**Outputs**:
- dataframe with GEOID and average life expectency
- saves dataframe as csv

**Updating Data**: The input dataset does not use the standard FIPS or GEOID to reference geography. The work around for this requires join to county data and some very specific text formating. Hopefully they transistion to standard unique ids in the future, but be careful regardless.

**Other Considerations**: While not present in this function. Life expectency is the only measure were a high value is better then a low value. We account for this in the `sensitivePopulations` function.


---

**Name**: lowBirthWeight.r

**Function Name**: `getLowBirthWeight()`

**Description**: Aggregates low birth weight measured at the census tract level to county and census block groups.

**Inputs**:
- filePath
- geometry
- processingLevel
- version
- overwrite


**helper function**: NA

**Outputs**:
- dataframe with GEOID and low birth weight percentage
- saves dataframe as csv

**Updating Data**: The code base adds a leading zero to TRACT_FIPS column. This seems to be coming from the input dataset not the function used to read in the data. When updating the dataset ensure that the input values are behaving the same way. Saved a numeric rather then factor or character values.

**Other Considerations**: NA

---

**Name**: mining.r

**Function Name**: `getmines()`

**Description**: Performs a distance weight measure of three input dataset to produce a single mine measure.

**Inputs**:
- geometry
- processingLevel
- version
- overwrite


**helper function**: bufferObjects

**Outputs**:
- dataframe with GEOID and distance weight measure for mining activity.
- saves dataframe as csv

**Updating Data**: Temporal filtering is done to the input dataset before reading in content.

**Other Considerations**: NA


---

**Name**: noise.r

**Function Name**: `getNoise()`

**Description**: Calcualtes the average noise value for each geography.

**Inputs**:
- filePath
- geometry
- processingLevel
- version
- overwrite


**helper function**: NA

**Outputs**:
- dataframe with GEOID and distance weight measure for mining activity.
- saves dataframe as csv

**Updating Data**: If they keep it as a raster feature any updates should be very easy to impliment.

**Other Considerations**: NA


---

**Name**: otherHaps.r

**Function Name**: `getOtherHAPS()`

**Description**: Normalized, distance weight measure of individual pollulations averaged across all point soure locations.

**Inputs**:
- filePath
- geometry
- processingLevel
- version
- overwrite


**helper function**: normalizeVector, bufferObjects

**Outputs**:
- dataframe with GEOID and distance weight measure for mining activity.
- saves dataframe as csv

**Updating Data**: Standard datasource so updates should be easy.

**Other Considerations**: Method is the same used in the `getHAPS` function but the pollutants are different. Temporal filtering is done n the input dataset. Iterative process is applied to the census block level analysis due to the long run times associated with the calculations.


---

**Name**: ozone.r

**Function Name**: `getOzone()`

**Description**: Values measured at the census tract level are aggregated to county and census block group levels.

**Inputs**:
- filePath
- geometry
- processingLevel
- version
- overwrite


**helper function**: NA

**Outputs**:
- dataframe with GEOID and measure of yearly ozone concentrations
- saves dataframe as csv

**Updating Data**: Standard datasource so updates should be easy.

**Other Considerations**: Input dataset is huge. We are storing it in the tar.gz file format it was delivered in. The `vroom` library can read this in. Could be packaged different in the future.


---

**Name**: placesData.r

**Function Name**: `getPlacesData()`

**Description**: Values measured at the census tract level are aggregated to county and census block group levels.

**Inputs**:
- filePath
- geometry
- processingLevel
- version
- overwrite


**helper function**: NA

**Outputs**:
- dataframe with GEOID and cancer, mentalHealth, and diabetes measures.
- saves dataframe as csv

**Updating Data**: Standard data source so updates should be easy.

**Other Considerations**: NA

---

**Name**: pm25.r

**Function Name**: `getPlacesData()`

**Description**: Values measured at the census tract level are aggregated to county and census block group levels.

**Inputs**:
- filePath
- geometry
- processingLevel
- version
- overwrite


**helper function**: NA

**Outputs**:
- dataframe with GEOID and average yearly pm2.5 values.
- saves dataframe as csv

**Updating Data**: Standard data source so updates should be easy.

**Other Considerations**: Input dataset is huge. We are storing it in the tar.gz file format it was delivered in. The `vroom` library can read this in. Could be packaged different in the future.



---

**Name**: proxyOilGas.r

**Function Name**: `getProxyOilGas()`

**Description**: Combines five input datasets and calculated a distance weight score value.

**Inputs**:
- geometry
- processingLevel
- version
- overwrite


**helper function**: bufferObjects

**Outputs**:
- dataframe with GEOID and average yearly pm2.5 values.
- saves dataframe as csv

**Updating Data**: Standard data source so updates should be easy. Need to update file paths to new input data directly within the function.

**Other Considerations**: Iterative process is applied to generate the census block group meausres. Overall computationally intensive due to the number of oil and gas locations.

---

**Name**: surfaceWater.r

**Function Name**: `getSurfaceWater()`

**Description**: Combines impared surface water measures and the realtive number of evaluated streams to generate a single score value.

**Inputs**:
- filePath
- geometry
- processingLevel
- version
- overwrite


**helper function**: bufferObjects

**Outputs**:
- dataframe with GEOID and impaired surface water measure.
- saves dataframe as csv

**Updating Data**: Standard data source so updates should be easy.

**Other Considerations**: Slightly complicated methodology please see technical document for more description on how score value came to be.

---

**Name**: wildfire.r

**Function Name**: `getWildfire()`

**Description**: Calculates average wildfire risk value for all geographies.

**Inputs**:
- filePath
- geometry
- processingLevel
- version
- overwrite


**helper function**: NA

**Outputs**:
- dataframe with GEOID and average wildfire risk.
- saves dataframe as csv

**Updating Data**: Standard data source so updates should be easy.

**Other Considerations**: NA



### compiling functions

#### standard inputs

Not all functions require each input.

**geometry**: Spatial object representing the geography of interest. Required by many individual data indicator processing functions.

**ejscreen**: A dataframe of all the EJScreen values. Generated as part of the `processData` function

**acsData**: A dataframe of all the ACS values. Generated as part of the `processData` function

**processingLevel**: character vector describing the geography of interest ("county", "censusTract", "censusBlockGroup"). Generally only used to constructing file paths.

**version**: character value used to represent the version of the enviroscreen dataset. A new version will force all data processings scripts to regenerate content.

**overwrite**: Binary TRUE/FALSE to define if a existing file should be overwriten. IF FALSE the existing file will be read in and returned as a dataframe.


- climate.r

---

**Name**: climate.r

**Function Name**: `climate()`

**Description**: Render four indicator processing functions and compiles to produce climate score .

**Inputs**:
- geometry
- processingLevel
- version
- overwrite


**helper function**: gm_mean

**Outputs**:
- dataframe with GEOID, measures and percentile values for all input parameters, and climate score values.
- saves dataframe as csv

**Updating Data**: Set overwrite to true or change version to force overwrite of score value.

**Other Considerations**: NA

---

**Name**: environmentalExposures.r

**Function Name**: `environmentalExposures()`

**Description**: Render seven indicator processing functions and compiles to produce environmental exposures score .

**Inputs**:
- geometry
- ejscreen
- processingLevel
- version
- overwrite


**helper function**:  gm_mean

**Outputs**:
- dataframe with GEOID, measures and percentile values for all input parameters, and environmental exposures score values.
- saves dataframe as csv

**Updating Data**: Set overwrite to true or change version to force overwrite of score value.

**Other Considerations**: This is a time intensive function to run. Largerly due to the buffering methods that is part of the haps and other haps calculations. If you are troubleshooting something in the joinDataFrames function utilize with health and population compoment score input not this one.


---

**Name**: environmentalEffects.r

**Function Name**: `enviromentalEffects()`

**Description**: Render four indicator processing functions and compiles to produce environmental effects score .

**Inputs**:
- geometry
- ejscreen
- processingLevel
- version
- overwrite


**helper function**:  gm_mean

**Outputs**:
- dataframe with GEOID, measures and percentile values for all input parameters, and environmental effects score values.
- saves dataframe as csv

**Updating Data**: Set overwrite to true or change version to force overwrite of score value.

**Other Considerations**: This is the time intensive function to run. Specifically at the census block group level. If you are troubleshooting something in the joinDataFrames function utilize with health and population component score input not this one.



---

**Name**: finalComponentScore.r

**Function Name**: `enviromentalEffects()`

**Description**: Take dataframes from all five component score and calculated group component score and final score values.

**Inputs**:
- dataframes


**helper function**: NA

**Outputs**:
- dataframe with GEOID, measures and percentile values for all measure used in the enviroscreen score.

**Updating Data**: Set overwrite to true or change version to force overwrite of score value.

**Other Considerations**: NA

---

**Name**: getShinyData.r

**Function Name**: `getShinyData()`

**Description**: Compiles data for the primary input dataset for the shiny applications.

**Inputs**:
- removeNativeLands
- removeZeroPop
- version
- spanish

**helper function**: NA

**Outputs**:
- RDS file with all information needed to render tables and indicator map visualizations within the shiny app.

**Updating Data**: Set overwrite to true or change version to force overwrite of score value.

**Other Considerations**: The output RDS file from this function defines a lot of the object names within the English version of the shiny application. We utilize a large `select` call to set column names. In the spanish version of the application we repeat this process to ensure no errors with special characters. The clause for spanish/english define what naming convention is used for the output data.


---

**Name**: joinDataFrames.r

**Function Name**: `joinDataFrames()`

**Description**: Compiles multiple dataframes into one and calculates the percentile rank for all features.

**Inputs**:
- dataframes

**helper function**: NA

**Outputs**:
- returns a single dataframe.

**Updating Data**: NA

**Other Considerations**: This functions does a lot and works on some big assumptions to do so. It's sensitive to the data type of input value and also utilizes some numeric column indexes. This is fine because there is a lot of structure organizing the data before it get to this point. That said, if changes are made to individual indicators or component score calculations I would expect errors to get to this function before showing up.


---

**Name**: processData.r

**Function Name**: `processData()`

**Description**: Controls all the data processing efforts.

**Inputs**:
- processingLevel
- version
- overwrite

**helper function**: setSpatialData

**Outputs**:
- returns a dataframe with all enviroscreen score values.
- writes out 8 csv from second level functions.

**Updating Data**: Set overwrite to true or change version to force overwrite of score value.

**Other Considerations**: Top level data processing function. If everything works it should be the only you have engage with to regenerate score values.


---

**Name**: sensitivePopulations.r

**Function Name**: `sensitivePopulations()`

**Description**: Renders six indicator processing functions and compiles to produce sensitive populations score.

**Inputs**:
- geometry
- processingLevel
- ejscreen
- version
- overwrite

**helper function**: gm_mean

**Outputs**:
- returns a dataframe with all sensitive population score values.

**Updating Data**: Set overwrite to true or change version to force overwrite of score value.

**Other Considerations**: Includes a few life for inverting the life expectancy score so that high values are considered good. This function renders very quickly.

---


**Name**: socioEconomic.r

**Function Name**: `socioEconomic()`

**Description**: Renders three indicator processing functions and compiles to produce socioeconomic score.

**Inputs**:
- geometry
- processingLevel
- acsData
- ejscreen
- version
- overwrite

**helper function**: gm_mean

**Outputs**:
- returns a dataframe with all sensitive population score values.

**Updating Data**: Set overwrite to true or change version to force overwrite of score value.

**Other Considerations**: This function renders very quickly.


### helper functions


**Name**: createFolderStructure.r

**Function Name**: `createFolderStructure()`

**Description**: Generates a folder structure require for effectively storing and indexing intemediate products from the data processing code.

**Inputs**:
- fileLocation : by default this is set to null and will be assigned to the R project library.

**helper function**: NA

**Outputs**:
- numerous file folders and directories

**Updating Data**: NA

**Other Considerations**: NA

---

**Name**: getGeometryLayers.r

**Function Name**: `getGeometryLayers()`

**Description**: Pulls geometry dataset from tigris or reads in existing features

**Inputs**:
- overwrite : by default this is set to FALSE as input geometry features are not expected to change.

**helper function**: NA

**Outputs**:
- geojson files for all require geographies

**Updating Data**: NA

**Other Considerations**: NA

---

**Name**: gm_mean.r

**Function Name**: `gm_mean()`

**Description**: Calculates the geometric mean of the numeric vector.

**Inputs**:
- x : a vector of numeric values

**helper function**: NA

**Outputs**:
- geometric mean as a numeric value.

**Updating Data**: NA

**Other Considerations**: NA values and zero values can not be passed into the geometric mean calculation due to it's use of natural log.


---

**Name**: loadFunctions.r

**Function Name**: `loadFunctions()`

**Description**: Sources all functions into global environment.

**Inputs**:
- NA

**helper function**: NA

**Outputs**:
- NA

**Updating Data**: NA

**Other Considerations**: This is a helpful function to use when you are troubleshooting a specific processing function. Do you edits, save, then type `loadFucntions()` into the console. Save some time swtiching back to the 0_main.R script to source the edited content.


---

**Name**: normalizeVector.r

**Function Name**: `normalizeVector()`

**Description**: Normalizes a numeric vector.

**Inputs**:
- x : numeric vector

**helper function**: NA

**Outputs**:
- a numeric vector

**Updating Data**: NA

**Other Considerations**: It was very odd for us to not find a built in R function for this process. This version does account for NA within the vectors. We use it on the HAPS and otherHAPS dataprocessing.


---

**Name**: patternLayer.r

**Function Name**: `patternLayer()`

**Description**: Generates the line filled polygons used in the visualization of coal, rural, and oil and gas communities within the shiny applications.

**Inputs**:
- NA

**helper function**: NA

**Outputs**:
- sf object

**Updating Data**: NA

**Other Considerations**: This has been a worrisome point for use in the development of this enviroscreen tool. We did not write this functions and it's not something we have the ability to alter without significant effort. We've kept it because it's only providing a visualization, and people liked the visualization. Effectively the function takes a polygon object and creates a new polygon object that have lines running through it. It's not a fill. Very cleaver work overall, see code for reference to the creator.

---

**Name**: setSpatialData.r

**Function Name**: `setSpatialData()`

**Description**: Reads in specific geojson to use for the geometry object.
**Inputs**:
- processingLevel

**helper function**: NA

**Outputs**:
- sf object

**Updating Data**: NA

**Other Considerations**: NA