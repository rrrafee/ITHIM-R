---
output:
  html_document: default
  pdf_document: default
---
# ITHIM-R

Development of the ITHIM-R, also known as ITHIM version 3.0. Started in January 2018.

This document aims to be a comprehensive record of the calculations in the ITHIM pipeline, specifically the ITHIM- R package. Some details here (and the default inputs to the functions) are specific to the Accra version of that model, as Accra has been the setting for construction of the prototype.

### Outline
In this document, in general, lower-case letters correspond to indices, or dimensions, of objects, and take one of a set of possible values, detailed in Table 1 (Convert from LATEX).

The set of fixed input data items are denoted by capital letters, and variable parameters are denoted by greek letters. These are tabulated in Table 2 (Convert from LATEX).


### Data inputs
ITHIM-R requires 5 user defined input files in csv format, saved in a directory of the city's name. See inst/ext/local/accra for example files. There are also numerous assumptions that you can parameterize in the model. 

#### File inputs
  * Travel survey (trips_CITY.csv) - a table of all trips taken by a group of people on a given day. Includes people who take no trips.
      * One row per trip (or stage of trip)
      * Minimal columns: participant_id, age, sex, trip_mode, trip_duration (or trip_distance)
      * Other columns: stage_mode, stage_duration (or stage_distance)
  * Recorded injury events (injuries_CITY.csv) - a table of recorded road-traffic injury (fatality) events in a city in one or more years.
      * One row per event
      * Minimal columns: cas_mode and strike_mode
      * Other columns: year, cas_age, cas_gender, weight (e.g. multiple years combined)
  * Disease burden data (gbd_CITY.csv)
      * One row per disease/metric/age/gender combination
      * Minimal rows: Measure (death/YLL); sex_name (Male/Female); age_name ('x to y'); Cause_name (disease names); Val (value of burden); population (number of people Val corresponds to, e.g. population of country)
  * Population of city (population_CITY.csv) - in order to scale the burden in Disease burden data to the city under study
      * One row per demographic group
      * Columns: sex, age, population
      * age column should share boundaries with age_name in Disease burden data, but can be more aggregated
  * Physical activity survey (pa_CITY.csv)
      * One row per person
      * Columns: sex, age, work_ltpa_marg_met = total leisure and work PA in a week
      
#### Function-call inputs

The following values are set in the call to the set-up function (run_ithim_setup).

  * Values for navigation:
      * CITY - the name of the city, which is also the name of the directory contain the 5 files
      * setup_call_summary_filename
      * PATH_TO_LOCAL_DATA
  * City-specific values
      * speeds
      * emission_inventory
  * Model parameters
      * DIST_CAT
      * AGE_RANGE
      * ADD_WALK_TO_BUS_TRIPS
      * ADD_BUS_DRIVERS
      * ADD_TRUCK_DRIVERS
      * TEST_WALK_SCENARIO
      * TEST_CYCLE_SCENARIO
      * MAX_MODE_SHARE_SCENARIO
      * REFERENCE_SCENARIO
      * NSAMPLES

The following values can be uncertain - i.e. they can be sampled from pre-specificied distributions, and the model run multiple (NSAMPLES) times, in order to evaluate the output with varying inputs.

  * Pollution values:
      * PM_CONC_BASE - lognormal - background PM2.5 concentration
      * PM_TRANS_SHARE - beta - proportion of background PM2.5 attributable to transport
      * EMISSION_INVENTORY_CONFIDENCE - value between 0 and 1 - how confident we are about the emission inventory
  * PA values:
      * BACKGROUND_PA_SCALAR - lognormal - scalar for physical activity
      * BACKGROUND_PA_CONFIDENCE - value between 0 and 1 - how confident we are about the PA survey (in terms of the number of people who report 0 PA)
      * MMET_CYCLING - lognormal - mMET value associated with cycling
      * MMET_WALKING - lognormal - mMET value associated with walking
  * Travel values:
      * BUS_WALK_TIME - lognormal - time taken to walk to PT
      * DAY_TO_WEEK_TRAVEL_SCALAR - beta - how daily travel scales to a week
      * BUS_TO_PASSENGER_RATIO - beta - number of buses per passenger
      * TRUCK_TO_CAR_RATIO - beta - number of trucks per car
      * DISTANCE_SCALAR_CAR_TAXI - lognormal - scalar for car/taxi distance
      * DISTANCE_SCALAR_WALKING - lognormal - scalar for walking distance
      * DISTANCE_SCALAR_PT - lognormal - scalar for PT distance
      * DISTANCE_SCALAR_CYCLING - lognormal - scalar for cycling distance
      * DISTANCE_SCALAR_MOTORCYCLE - lognormal - scalar for motorcycle distance
  * Health values:
      * PA_DOSE_RESPONSE_QUANTILE - logic - whether or not to sample from DR curves
      * AP_DOSE_RESPONSE_QUANTILE - logic - whether or not to sample from DR curves
      * CHRONIC_DISEASE_SCALAR - lognormal - scalar for GBD data
  * Injury values:
      * INJURY_REPORTING_RATE - lognormal - the rate at which injuries are reported
      * INJURY_LINEARITY - lognormal - how linear injuries are in space
      * CASUALTY_EXPONENT_FRACTION - beta - fraction of injury linearity attributed to casualty mode
      
#### Synthetic Population

**Description** 
The first input you will need to provide is the synthetic population data. This data typically comes from a household travel survey or travel time use survey, and a self-report leisure time physical activity survey. These data will be uses throughout the process. 

**Synthetic Population Dataset Format**
You should have a table with the following variables

* trip_id
* trip_mode
* trip_duration
* participant_id
* age
* sex
* age_cat
* ltpa_marg_met
* work_marg_met

**Montreal Synthetic Population Dataset Example**
```{r}


```

#### Who Hit Who Matrix

**Description**
TEXT HERE

**Who Hit Who Matrix Dataset Format**
A matrix 

**Montreal Who Hit Who Matrix Dataset Example**
```{r}


```


### Air Pollution

**Description**
TEXT HERE

**Air Pollution Dataset Format**
A matrix 

**Montreal Air Pollution Dataset Example**
```{r}


```

**We are currently working on developing a separate package to create a synthetic population**  


For further documentation consult the wiki. 
For ongoing discussions, see issues.
In addition, relevant documents are stored in shared GDrive folder, and a Slack channel is available for communications among contributors.

See [communication channels on Wiki](https://github.com/ITHIM/ITHIM-R/wiki/Communication-channels)

## Related Repositories 
* [ITHIM (R, Physical Activity)](https://github.com/ITHIM/ITHIM)
* [ITHIM Interface (R, Shiny)](https://github.com/ITHIM/ithim-r-interface)
