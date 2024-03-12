# Ford GoBike System Data Analysis

## Table of Contents
1. [Overview](#overview)
2. [Dataset Summary](#dataset-summary)
3. [Data Preparation Steps](#data-preparation-steps)
4. [Analysis Highlights](#analysis-highlights)
5. [Conclusions](#conclusions)
6. [Submission Details](#submission-details)

## Overview
This analysis investigates the Ford GoBike bikeshare data to uncover patterns and correlations among users. Specifically, it focuses on the relationships between a rider's age and their ride duration, compares behaviors of subscribers and customers, and examines the impact of gender on these variables.

## Dataset Summary
The dataset encompasses single-trip data within the San Francisco Bay area from 2019, detailing rental locations, durations, and user-specific information like birth year, user type, and gender.

## Data Preparation Steps
Below are the steps taken to prepare the data for analysis. The Python code used for these steps includes importing necessary libraries, loading the dataset, and cleaning the data.

```python
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
import warnings

warnings.simplefilter("ignore")

# Load dataset
fordgobike_df = pd.read_csv('201902-fordgobike-tripdata.csv')

# Data cleaning
fordgobike_df.dropna(inplace=True)
fordgobike_df.replace([np.inf, -np.inf], np.nan, inplace=True)
fordgobike_df.dropna(inplace=True)
fordgobike_df = fordgobike_df.astype({'member_birth_year': 'int32', 'start_station_id': 'int32', 'end_station_id': 'int32'})

# Data type conversions and categorization
ordered_categories = {'user_type': ['Subscriber', 'Customer'],
                      'member_gender': ['Male', 'Female', 'Other'],
                      'bike_share_for_all_trip': ['Yes', 'No']}

for variable in ordered_categories:
    ordered_var = pd.api.types.CategoricalDtype(ordered=True, categories=ordered_categories[variable])
    fordgobike_df[variable] = fordgobike_df[variable].astype(ordered_var)

fordgobike_df['start_time'] = pd.to_datetime(fordgobike_df['start_time'])
fordgobike_df['start_day_of_week'] = fordgobike_df['start_time'].dt.day_name()
days_of_week = ["Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday", "Sunday"]
fordgobike_df['start_day_of_week'] = pd.Categorical(fordgobike_df['start_day_of_week'], categories=days_of_week, ordered=True)
fordgobike_df['rider_age'] = 2019 - fordgobike_df['member_birth_year']
