# US Domestic Flight Delays: September to December 2025


# Background

The 2025 federal government shutdown ran from October 1 to November 12.
It was the longest in US history. During this time air traffic
controllers worked without pay throughout. The FAA ordered airlines to
cut flights at 40 major US airports from November 7 until the order was
lifted on November 17.

# The Data

The data reflects all delayed domestic flights with in the US for the
September–December 2025 period. This covers a normal month before the
shutdown, the shutdown itself, the FAA-mandated cuts and the holiday
recovery.

# The Data Source

The dataset was found on Kaggle and sourced from the official US Bureau
of Transportation Statistics (BTS) Reporting Carrier On-Time Performance
Database.

# Load Libraries

``` python
import pandas as pd
```

# Import the Data

``` python
sep = pd.read_csv("~/Documents/MSBAAcademics/Fall_Mod1/DataWrangling/FinalProject/Data/flight_delays_2025_09.csv")

oct = pd.read_csv("~/Documents/MSBAAcademics/Fall_Mod1/DataWrangling/FinalProject/Data/flight_delays_2025_10.csv")

nov = pd.read_csv("~/Documents/MSBAAcademics/Fall_Mod1/DataWrangling/FinalProject/Data/flight_delays_2025_11.csv")

dec = pd.read_csv("~/Documents/MSBAAcademics/Fall_Mod1/DataWrangling/FinalProject/Data/flight_delays_2025_12.csv")
```

# Initial Exploration

To explore the dfs with forloops - add them to a dictionary

``` python
dfs = {"September": sep, "October": oct, "November": nov, "December": dec}
```

Check the shape of all 4 dataframes?

``` python
for i, df in dfs.items():
    print(f"{i}: {df.shape}")
```

    September: (562439, 40)
    October: (605844, 40)
    November: (570550, 40)
    December: (582304, 40)

Each dataframe has 40 columns

Print the column names of September

``` python
sep.columns.to_list
```

    <bound method IndexOpsMixin.tolist of Index(['Year', 'Quarter', 'Month', 'DayofMonth', 'DayOfWeek', 'FlightDate',
           'Reporting_Airline', 'Flight_Number_Reporting_Airline', 'Origin',
           'OriginCityName', 'OriginState', 'Dest', 'DestCityName', 'DestState',
           'CRSDepTime', 'DepTime', 'DepDelay', 'DepDelayMinutes', 'DepDel15',
           'TaxiOut', 'WheelsOff', 'WheelsOn', 'TaxiIn', 'CRSArrTime', 'ArrTime',
           'ArrDelay', 'ArrDelayMinutes', 'ArrDel15', 'Cancelled',
           'CancellationCode', 'Diverted', 'CRSElapsedTime', 'ActualElapsedTime',
           'AirTime', 'Distance', 'CarrierDelay', 'WeatherDelay', 'NASDelay',
           'SecurityDelay', 'LateAircraftDelay'],
          dtype='str')>

Do the column names match for each month? Use Sep as the reference.

``` python
ref_cols = list(dfs["September"].columns)

for i, df in dfs.items():
    print(i, list(df.columns) == ref_cols)
```

    September True
    October True
    November True
    December True

Do the data types match?

``` python
ref_types = list(dfs["September"].dtypes)

for i, df in dfs.items():
    print(i, list(df.dtypes) == ref_types)
```

    September True
    October True
    November True
    December True

# Join the Data: Stack on top

``` python
flights = pd.concat([sep, oct, nov, dec], ignore_index=True)

print(flights.shape)
```

    (2321137, 40)

``` python
flights.head
```

    <bound method NDFrame.head of          Year  Quarter  Month  DayofMonth  DayOfWeek  FlightDate  \
    0        2025        3      9           6          6  2025-09-06   
    1        2025        3      9           7          7  2025-09-07   
    2        2025        3      9           8          1  2025-09-08   
    3        2025        3      9          10          3  2025-09-10   
    4        2025        3      9          11          4  2025-09-11   
    ...       ...      ...    ...         ...        ...         ...   
    2321132  2025        4     12          12          5  2025-12-12   
    2321133  2025        4     12          12          5  2025-12-12   
    2321134  2025        4     12          12          5  2025-12-12   
    2321135  2025        4     12          12          5  2025-12-12   
    2321136  2025        4     12          12          5  2025-12-12   

            Reporting_Airline  Flight_Number_Reporting_Airline Origin  \
    0                      MQ                             3447    ORD   
    1                      MQ                             3447    ORD   
    2                      MQ                             3447    ORD   
    3                      MQ                             3447    ORD   
    4                      MQ                             3447    ORD   
    ...                   ...                              ...    ...   
    2321132                WN                             1393    MDW   
    2321133                WN                             2594    MDW   
    2321134                WN                             2952    MDW   
    2321135                WN                             3727    MDW   
    2321136                WN                             3732    MDW   

            OriginCityName  ... Diverted CRSElapsedTime ActualElapsedTime AirTime  \
    0          Chicago, IL  ...      0.0          100.0              83.0    48.0   
    1          Chicago, IL  ...      0.0           94.0              78.0    50.0   
    2          Chicago, IL  ...      0.0           94.0              77.0    47.0   
    3          Chicago, IL  ...      0.0           94.0              82.0    52.0   
    4          Chicago, IL  ...      0.0           94.0              88.0    61.0   
    ...                ...  ...      ...            ...               ...     ...   
    2321132    Chicago, IL  ...      0.0          155.0             162.0   127.0   
    2321133    Chicago, IL  ...      0.0          155.0             149.0   127.0   
    2321134    Chicago, IL  ...      0.0          155.0             134.0   119.0   
    2321135    Chicago, IL  ...      0.0          160.0             146.0   120.0   
    2321136    Chicago, IL  ...      0.0          155.0             140.0   122.0   

             Distance  CarrierDelay  WeatherDelay  NASDelay  SecurityDelay  \
    0           296.0           NaN           NaN       NaN            NaN   
    1           296.0           NaN           NaN       NaN            NaN   
    2           296.0          35.0           0.0       0.0            0.0   
    3           296.0           NaN           NaN       NaN            NaN   
    4           296.0           9.0           0.0       0.0            0.0   
    ...           ...           ...           ...       ...            ...   
    2321132     990.0           NaN           NaN       NaN            NaN   
    2321133     990.0           NaN           NaN       NaN            NaN   
    2321134     990.0           NaN           NaN       NaN            NaN   
    2321135     990.0           NaN           NaN       NaN            NaN   
    2321136     990.0           NaN           NaN       NaN            NaN   

             LateAircraftDelay  
    0                      NaN  
    1                      NaN  
    2                      0.0  
    3                      NaN  
    4                     19.0  
    ...                    ...  
    2321132                NaN  
    2321133                NaN  
    2321134                NaN  
    2321135                NaN  
    2321136                NaN  

    [2321137 rows x 40 columns]>
