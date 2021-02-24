# Education Data Portal - Summary Endpoints

These summary endpoints extend the functionality of the Urban Institute's 
[Education Data Portal](https://educationdata.urban.org) application 
programming interface (API). Where the original API allows researchers to 
easily access millions of raw records for further analysis, the summary 
endpoints provide rapid, customized access to aggregated 
statistics in seconds for policymakers, journalists, and community groups. 

For more information, see the full Education Data Portal 
[documentation page](https://educationdata.urban.org/documentation/index.html#summary_endpoints).

We appreciate all feedback, which can either 
be filed as an issue in this GitHub repository or sent to: educationdata@urban.org.

For additional background, see our:

* Data@Urban blog post from January 2019:
[Democratizing Big Data to Support Proactive Policymaking](https://medium.com/@urban_institute/data-your-fingertips-democratizing-big-data-to-support-proactive-policymaking-fb7cc9d4a0aa).
* And an update from February 2021: [Building Summary Endpoints for the Education Data Portal](https://urban-institute.medium.com/building-summary-endpoints-for-the-education-data-portal-139eb7f34fc6)

## User Guide

### Intro

The general form of an API call is as follows:

`educationdata.urban.org/api/v1/{section}/{source}/{topic}/summaries?var={var}&stat={stat}&by={by}`

Where:

* `{topic}` is one of `college-university`, `school-districts`, or `schools`.
* `{source}` is the data source, i.e. `ccd`, `crdc`, `ipeds`, `edfacts`, 
`saipe`, or `scorecard`.
* `{endpoint}` is the endpoint, i.e. `enrollment`, `ap-exams`, etc.
* `{var}` is the variable to be summarized. Must be a numeric, non-filter 
variable.
* `{stat}` is the statistic to be calculated. Valid options are:
  - `sum` 
  - `count`
  - `avg`
  - `min`
  - `max`
  - `variance`
  - `stddev`
  - `median` (note that this is mapped as an 
  `approx_percentile(var, 0.50)` query)
* `{by}` is the variable(s) to group results by. Multiple variables can be 
given as a comma separated string, i.e. `by=fips,race`. All queries are also 
automatically grouped by `year` by default.

### Example Use

A simple call will look like:

https://educationdata.urban.org/api/v1/schools/ccd/enrollment/summaries?var=enrollment&stat=sum&by=fips

This will return the sum of `enrollment` by `fips` code and `year` for the 
`schools/ccd/enrollment` endpoint.

Where applicable, tables have been pre-joined against their associated 
`directory` file, providing additional options for `by` and `filter`
arguments. For example, we can call:

https://educationdata.urban.org/api/v1/schools/ccd/enrollment/summaries?var=enrollment&by=school_level&stat=sum 

even though `school_level` is only available in the `schools/ccd/directory` 
endpoint and not the `schools/ccd/enrollment` endpoint.

### Adding Filters

Additional filters can be added to the API call as additional query string 
parameters in the form `{filter}={value}`. Multiple values can be given as a 
comma separated string. For example:

https://educationdata.urban.org/api/v1/schools/ccd/enrollment/summaries?var=enrollment&stat=sum&by=sex&fips=6&race=1,2

will return the sum of enrollment by sex and year for the 
`schools/ccd/enrollment` endpoint where `fips` is 6 and `race` is either 1 or 
2.

### Additional Help

See the full 
[documentation](https://educationdata.urban.org/documentation/index.html) 
for the Education Data Portal for additional information on the project. 

In particular, the topic specific documentation may be useful when 
constructing a query:
  * [Schools](https://educationdata.urban.org/documentation/schools.html)
  * [School Districts](https://educationdata.urban.org/documentation/school-districts.html) 
  * [College-University](https://educationdata.urban.org/documentation/colleges.html)

## Additional Examples

* `schools/crdc/ap-exams`
  - Sum of `students_AP_exam_none` by `fips` code and `year` 
  (automatically added).
  - https://educationdata.urban.org/api/v1/schools/crdc/ap-exams/summaries?var=students_AP_exam_none&stat=sum&by=fips
* `schools/crdc/enrollment`
  - Sum of `enrollment_crdc` by `fips` code, `race`, and `year` 
  (automatically added), where `fips` is 6 or 8. 
  - https://educationdata.urban.org/api/v1/schools/crdc/enrollment/summaries?var=enrollment_crdc&stat=sum&by=fips,race&fips=6,8
* `schools/crdc/harassment-or-bullying/allegations`
  - Sum of `allegations_harass_sex` by `fips`, `g1`, and `year` 
  (automatically added) where year is 2013.
  - https://educationdata.urban.org/api/v1/schools/crdc/harassment-or-bullying/allegations/summaries?var=allegations_harass_sex&stat=sum&by=fips,g1&year=2013
* `schools/crdc/restraint-and-seclusion/instances`
  - Max of `instances_seclusion` by `magnet_crdc` and `year` 
  (automatically added) where `fips` is 51.
  - https://educationdata.urban.org/api/v1/schools/crdc/restraint-and-seclusion/instances/summaries?var=instances_seclusion&stat=max&by=magnet_crdc&fips=51
* `college-university/ipeds/completions-cip-6`
  - (Approximate) median of `awards` by `award_level` and 
  `year` (automatically added) where year is 2010 or 2011
  - https://educationdata.urban.org/api/v1/college-university/ipeds/completions-cip-6/summaries/?var=awards&by=award_level&stat=median&year=2010,2011