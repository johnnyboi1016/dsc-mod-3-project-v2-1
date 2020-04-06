
# Module 3 Final Project - Chicago Car Crashes

* Student name: John Cho
* Student pace: full time online
* Instructor name: Rafael Carrasco
* Blog post URL:


## From City of Chicago's Data Portal: [link](https://data.cityofchicago.org/Transportation/Traffic-Crashes-Crashes/85ca-t3if)
### Traffic Crashes - Crashes:
Crash data shows information about each traffic crash on city streets within the City of Chicago limits and under the jurisdiction of Chicago Police Department (CPD). Data are shown as is from the electronic crash reporting system (E-Crash) at CPD, excluding any personally identifiable information. Records are added to the data portal when a crash report is finalized or when amendments are made to an existing report in E-Crash. Data from E-Crash are available for some police districts in 2015, but citywide data are not available until September 2017. About half of all crash reports, mostly minor crashes, are self-reported at the police district by the driver(s) involved and the other half are recorded at the scene by the police officer responding to the crash. Many of the crash parameters, including street condition data, weather condition, and posted speed limits, are recorded by the reporting officer based on best available information at the time, but many of these may disagree with posted information or other assessments on road conditions. If any new or updated information on a crash is received, the reporting officer may amend the crash report at a later time. A traffic crash within the city limits for which CPD is not the responding police agency, typically crashes on interstate highways, freeway ramps, and on local roads along the City boundary, are excluded from this dataset.  

    All crashes are recorded as per the format specified in the Traffic Crash Report, SR1050, of the Illinois Department of Transportation. As per Illinois statute, only crashes with a property damage value of $1,500 or more or involving bodily injury to any person(s) and that happen on a public roadway and that involve at least one moving vehicle, except bike dooring, are considered reportable crashes. However, CPD records every reported traffic crash event, regardless of the statute of limitations, and hence any formal Chicago crash dataset released by Illinois Department of Transportation may not include all the crashes listed here.  

### Traffic Crashes - Vehicles: 
This dataset contains information about vehicles (or units as they are identified in crash reports) involved in a traffic crash. This dataset should be used in conjunction with the traffic Crash and People dataset available in the portal. “Vehicle” information includes motor vehicle and non-motor vehicle modes of transportation, such as bicycles and pedestrians. Each mode of transportation involved in a crash is a “unit” and get one entry here. Each vehicle, each pedestrian, each motorcyclist, and each bicyclist is considered an independent unit that can have a trajectory separate from the other units. However, people inside a vehicle including the driver do not have a trajectory separate from the vehicle in which they are travelling and hence only the vehicle they are travelling in get any entry here. This type of identification of “units” is needed to determine how each movement affected the crash. Data for occupants who do not make up an independent unit, typically drivers and passengers, are available in the People table. Many of the fields are coded to denote the type and location of damage on the vehicle. Vehicle information can be linked back to Crash data using the “CRASH_RECORD_ID” field. Since this dataset is a combination of vehicles, pedestrians, and pedal cyclists not all columns are applicable to each record. Look at the Unit Type field to determine what additional data may be available for that record.  

### Traffic Crashes - People: 
This data contains information about people involved in a crash and if any injuries were sustained. This dataset should be used in combination with the traffic Crash and Vehicle dataset. Each record corresponds to an occupant in a vehicle listed in the Crash dataset. Some people involved in a crash may not have been an occupant in a motor vehicle, but may have been a pedestrian, bicyclist, or using another non-motor vehicle mode of transportation. Injuries reported are reported by the responding police officer. Fatalities that occur after the initial reports are typically updated in these records up to 30 days after the date of the crash. Person data can be linked with the Crash and Vehicle dataset using the “CRASH_RECORD_ID” field. A vehicle can have multiple occupants and hence have a one to many relationship between Vehicle and Person dataset. However, a pedestrian is a “unit” by itself and have a one to one relationship between the Vehicle and Person table.

## Data Cleaning, Exploration, Transformation
This portion of the project lived up to the cliche we are so familiar with: cleaning and prep work before modeling takes up 80% of a data scientist's time. This was definitely true for me this project, despite having a dataset already fairly 'clean' and updated near daily by the City of Chicago.

I adopted a balanced mindset of simplifying features and their values in regards to how different events, elements, values should be separated when examined as a contributing factor to a severe crash. Boiling down a large spread of values to a few with relative significance behind them would vary wildly depending on the eye of the beholder. This is where a data scientist's domain knowledge and personal biases could heavily influence how this part of the process is performed. In the end, being able to explain and understand why a feature was changed, removed or added is paramount to properly interpreting the modeling results.

## Final list of curated features and binned values:

`POSTED_SPEED_LIMIT` - low: 0-15mph; med: 16-45; high: 46+ mph

`WEATHER_CONDITION` - CLEAR, CLOUDY/RAIN, SNOW/ICY, UNKNOWN/OTHER, FOG/SMOKE/HAZE, SEVERE_WINDS

`LIGHTING_CONDITION` - DAYLIGHT, (DARKNESS, LIGHTED ROAD), DARKNESS, DUSK, DAWN

`FIRST_CRASH_TYPE` - PARKED/FIXED, TURN/ANGLE, REAR/NONCOL, SIDESWIPE, PED/CYCLE/OTHER, HEAD/OVER/TRAIN

`LANE_CNT` - -1: null/unknown, 0: intersection, 1, 2, 3, 4, 5, 6: six lanes or more

`ALIGNMENT` - (of the road) STRAIGHT, CURVE, HILLCREST

`ROADWAY_SURFACE_COND` - DRY, WET, UNKNOWN/OTHER, SNOW/ICE/SAND

`ROAD_DEFECT` - NO DEFECTS, UNKNOWN/OTHER/DEBRIS, WORN/SHOULDER/RUT

`REPORT_TYPE` = 0: not on scene, desk report; 1: on scene report

`HIT_AND_RUN_I` - 0: no; 1: yes

`DAMAGE` - OVER $1,500, $1500 OR LESS

`STREET_DIRECTION` - S/W (sunset), N/E (sunrise)

`NUM_UNITS` - unit=object with velocity; 1, 2, 3: three or more

`INJURIES_TOTAL` - 0: none; 1, 2, 3: three or more injuries

`CRASH_HOUR` - DAWN (5a-7a), DAY (10a-4p), DUSK (5p-7p), NIGHT (8p-4a)

`CRASH_DAY_OF_WEEK` - WORK (Mon-Thu), LEISURE (Fri-Sun)

`CRASH_MONTH` - COLD (Nov-Feb), MILD (Mar/Apr/May/Oct), HOT (Jun-Sep)

`POL_NOTIFY_TIME` - QUICK (0-10 minutes), LOW (11-30), MID (31-120), HIGH (120+ minutes)

`NO_TCD` - 0: no traffic control device, 1: TCD present

`NON_DRIVERS` - 0: no other people aside from drivers involved in crash, 1: passengers/pedestrians/bicyclists/etc. present

`OOSTATE`: 0: all drivers have an IL state license, 1: out of state drivers present

`WEIGHT_DIFF` - 0: only passenger cars (3-4000 lbs) involved, 1: at least one heavy (truck, bus) or light (motorcycle) vehicle present

`SPEEDING` - 0: no speeding suspected, 1: excessive speed determined to be contributing factor in crash

`UNDER25` - 0: all drivers are 25 and older, 1: at least one under 25 driver involved

`ALCOHOL` - 0: none suspected, 1: alcohol involvement suspected, sobriety test requested or administered

`AIRBAG` - 0: no airbags deployed, 1: at least one airbag deployed

`EJECTION` - 0: no people ejection/trapped, 1: at least one person ejected from vehicle / trapped in vehicle

## Modeling
We discussed the class imbalance present in this dataset due to the rarity of severe crashes (<2%). This imbalance presented itself in the variety of metric scores we observed for accuracy, precision, recall and F1 score. We explored Logistic Regression models, feature selection using the Chi-Squared Test, Decision Trees, Random Forests, boosting algorithms and attempted to build a Support Vector Machine model.

In the end, the least sophisticated (and quickest to build regarding CPU time) logistic regression model fared best in maximizing the metric we were looking for: recall. This is because in the real world business case of this project, predicting every actual severe crash as correctly as possible was more important than anything else. We achieved a 98% recall rate which came at the cost of precision, which lowered to 14% (highest observed precision score was 71% with AdaBoost).

In terms of thinking about the methodology each model used, it makes sense that a model based on probabilities was best suited to our real world application of a rare prediction riddled with noise, despite having such a large dataset. It goes to show that in machine learning models - bigger isn't always better and complex isn't always better than simple.

