# US Domestic Flight Delays: September to December 2025


# Background

The 2025 federal government shutdown ran from October 1 to November 12.
It was the longest in US history. During this time air traffic
controllers worked without pay throughout. The FAA ordered airlines to
cut flights at 40 major US airports from November 7 until the order was
lifted on November 17.

# 3 Airports of Interest

Chicago O’Hare (ORD) Atlanta (ATL) Dallas-Fort Worth (DFW)

According to Paul Hartley, in his article about which U.S. airports were
hit hardest by the mass flight cancellations during the government
shutdown. Link:
https://simpleflying.com/examined-which-airports-hardest-hit-mass-cancellations-shutdown/

# The Data

The data reflects all delayed domestic flights within the US for the
September–December 2025 period. This covers a normal month before the
shutdown, the shutdown itself, the FAA-mandated cuts and the holiday
recovery.

# The Data Source

The dataset was found on Kaggle and sourced from the official US Bureau
of Transportation Statistics (BTS) Reporting Carrier On-Time Performance
Database.

# Take note

Cancellation Code gives cancelation reason.  
A = carrier, B = weather, C = NAS, D = security. Empty if not cancelled.

Diverted: 1 if the flight was diverted to another airport, otherwise 0.

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
sep.columns.to_list()
```

    ['Year',
     'Quarter',
     'Month',
     'DayofMonth',
     'DayOfWeek',
     'FlightDate',
     'Reporting_Airline',
     'Flight_Number_Reporting_Airline',
     'Origin',
     'OriginCityName',
     'OriginState',
     'Dest',
     'DestCityName',
     'DestState',
     'CRSDepTime',
     'DepTime',
     'DepDelay',
     'DepDelayMinutes',
     'DepDel15',
     'TaxiOut',
     'WheelsOff',
     'WheelsOn',
     'TaxiIn',
     'CRSArrTime',
     'ArrTime',
     'ArrDelay',
     'ArrDelayMinutes',
     'ArrDel15',
     'Cancelled',
     'CancellationCode',
     'Diverted',
     'CRSElapsedTime',
     'ActualElapsedTime',
     'AirTime',
     'Distance',
     'CarrierDelay',
     'WeatherDelay',
     'NASDelay',
     'SecurityDelay',
     'LateAircraftDelay']

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

Look at the head & tail of the data (but see all 40 columns)

``` python
with pd.option_context('display.max_columns', None): display(flights.head())
```

<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }
&#10;    .dataframe tbody tr th {
        vertical-align: top;
    }
&#10;    .dataframe thead th {
        text-align: right;
    }
</style>

|  | Year | Quarter | Month | DayofMonth | DayOfWeek | FlightDate | Reporting_Airline | Flight_Number_Reporting_Airline | Origin | OriginCityName | OriginState | Dest | DestCityName | DestState | CRSDepTime | DepTime | DepDelay | DepDelayMinutes | DepDel15 | TaxiOut | WheelsOff | WheelsOn | TaxiIn | CRSArrTime | ArrTime | ArrDelay | ArrDelayMinutes | ArrDel15 | Cancelled | CancellationCode | Diverted | CRSElapsedTime | ActualElapsedTime | AirTime | Distance | CarrierDelay | WeatherDelay | NASDelay | SecurityDelay | LateAircraftDelay |
|----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|
| 0 | 2025 | 3 | 9 | 6 | 6 | 2025-09-06 | MQ | 3447 | ORD | Chicago, IL | IL | CMH | Columbus, OH | OH | 1845 | 1843.0 | -2.0 | 0.0 | 0.0 | 24.0 | 1907.0 | 2055.0 | 11.0 | 2125 | 2106.0 | -19.0 | 0.0 | 0.0 | 0.0 | NaN | 0.0 | 100.0 | 83.0 | 48.0 | 296.0 | NaN | NaN | NaN | NaN | NaN |
| 1 | 2025 | 3 | 9 | 7 | 7 | 2025-09-07 | MQ | 3447 | ORD | Chicago, IL | IL | CMH | Columbus, OH | OH | 1939 | 2002.0 | 23.0 | 23.0 | 1.0 | 20.0 | 2022.0 | 2212.0 | 8.0 | 2213 | 2220.0 | 7.0 | 7.0 | 0.0 | 0.0 | NaN | 0.0 | 94.0 | 78.0 | 50.0 | 296.0 | NaN | NaN | NaN | NaN | NaN |
| 2 | 2025 | 3 | 9 | 8 | 1 | 2025-09-08 | MQ | 3447 | ORD | Chicago, IL | IL | CMH | Columbus, OH | OH | 1939 | 2031.0 | 52.0 | 52.0 | 1.0 | 23.0 | 2054.0 | 2241.0 | 7.0 | 2213 | 2248.0 | 35.0 | 35.0 | 1.0 | 0.0 | NaN | 0.0 | 94.0 | 77.0 | 47.0 | 296.0 | 35.0 | 0.0 | 0.0 | 0.0 | 0.0 |
| 3 | 2025 | 3 | 9 | 10 | 3 | 2025-09-10 | MQ | 3447 | ORD | Chicago, IL | IL | CMH | Columbus, OH | OH | 1939 | 1934.0 | -5.0 | 0.0 | 0.0 | 22.0 | 1956.0 | 2148.0 | 8.0 | 2213 | 2156.0 | -17.0 | 0.0 | 0.0 | 0.0 | NaN | 0.0 | 94.0 | 82.0 | 52.0 | 296.0 | NaN | NaN | NaN | NaN | NaN |
| 4 | 2025 | 3 | 9 | 11 | 4 | 2025-09-11 | MQ | 3447 | ORD | Chicago, IL | IL | CMH | Columbus, OH | OH | 1939 | 2013.0 | 34.0 | 34.0 | 1.0 | 22.0 | 2035.0 | 2236.0 | 5.0 | 2213 | 2241.0 | 28.0 | 28.0 | 1.0 | 0.0 | NaN | 0.0 | 94.0 | 88.0 | 61.0 | 296.0 | 9.0 | 0.0 | 0.0 | 0.0 | 19.0 |

</div>

``` python
with pd.option_context('display.max_columns', None): display(flights.tail())
```

<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }
&#10;    .dataframe tbody tr th {
        vertical-align: top;
    }
&#10;    .dataframe thead th {
        text-align: right;
    }
</style>

|  | Year | Quarter | Month | DayofMonth | DayOfWeek | FlightDate | Reporting_Airline | Flight_Number_Reporting_Airline | Origin | OriginCityName | OriginState | Dest | DestCityName | DestState | CRSDepTime | DepTime | DepDelay | DepDelayMinutes | DepDel15 | TaxiOut | WheelsOff | WheelsOn | TaxiIn | CRSArrTime | ArrTime | ArrDelay | ArrDelayMinutes | ArrDel15 | Cancelled | CancellationCode | Diverted | CRSElapsedTime | ActualElapsedTime | AirTime | Distance | CarrierDelay | WeatherDelay | NASDelay | SecurityDelay | LateAircraftDelay |
|----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|
| 2321132 | 2025 | 4 | 12 | 12 | 5 | 2025-12-12 | WN | 1393 | MDW | Chicago, IL | IL | MCO | Orlando, FL | FL | 650 | 649.0 | -1.0 | 0.0 | 0.0 | 25.0 | 714.0 | 1021.0 | 10.0 | 1025 | 1031.0 | 6.0 | 6.0 | 0.0 | 0.0 | NaN | 0.0 | 155.0 | 162.0 | 127.0 | 990.0 | NaN | NaN | NaN | NaN | NaN |
| 2321133 | 2025 | 4 | 12 | 12 | 5 | 2025-12-12 | WN | 2594 | MDW | Chicago, IL | IL | MCO | Orlando, FL | FL | 1710 | 1704.0 | -6.0 | 0.0 | 0.0 | 14.0 | 1718.0 | 2025.0 | 8.0 | 2045 | 2033.0 | -12.0 | 0.0 | 0.0 | 0.0 | NaN | 0.0 | 155.0 | 149.0 | 127.0 | 990.0 | NaN | NaN | NaN | NaN | NaN |
| 2321134 | 2025 | 4 | 12 | 12 | 5 | 2025-12-12 | WN | 2952 | MDW | Chicago, IL | IL | MCO | Orlando, FL | FL | 1135 | 1140.0 | 5.0 | 5.0 | 0.0 | 11.0 | 1151.0 | 1450.0 | 4.0 | 1510 | 1454.0 | -16.0 | 0.0 | 0.0 | 0.0 | NaN | 0.0 | 155.0 | 134.0 | 119.0 | 990.0 | NaN | NaN | NaN | NaN | NaN |
| 2321135 | 2025 | 4 | 12 | 12 | 5 | 2025-12-12 | WN | 3727 | MDW | Chicago, IL | IL | MCO | Orlando, FL | FL | 805 | 801.0 | -4.0 | 0.0 | 0.0 | 11.0 | 812.0 | 1112.0 | 15.0 | 1145 | 1127.0 | -18.0 | 0.0 | 0.0 | 0.0 | NaN | 0.0 | 160.0 | 146.0 | 120.0 | 990.0 | NaN | NaN | NaN | NaN | NaN |
| 2321136 | 2025 | 4 | 12 | 12 | 5 | 2025-12-12 | WN | 3732 | MDW | Chicago, IL | IL | MCO | Orlando, FL | FL | 1935 | 1951.0 | 16.0 | 16.0 | 1.0 | 11.0 | 2002.0 | 2304.0 | 7.0 | 2310 | 2311.0 | 1.0 | 1.0 | 0.0 | 0.0 | NaN | 0.0 | 155.0 | 140.0 | 122.0 | 990.0 | NaN | NaN | NaN | NaN | NaN |

</div>

``` python
flights.info()
```

    <class 'pandas.DataFrame'>
    RangeIndex: 2321137 entries, 0 to 2321136
    Data columns (total 40 columns):
     #   Column                           Dtype  
    ---  ------                           -----  
     0   Year                             int64  
     1   Quarter                          int64  
     2   Month                            int64  
     3   DayofMonth                       int64  
     4   DayOfWeek                        int64  
     5   FlightDate                       str    
     6   Reporting_Airline                str    
     7   Flight_Number_Reporting_Airline  int64  
     8   Origin                           str    
     9   OriginCityName                   str    
     10  OriginState                      str    
     11  Dest                             str    
     12  DestCityName                     str    
     13  DestState                        str    
     14  CRSDepTime                       int64  
     15  DepTime                          float64
     16  DepDelay                         float64
     17  DepDelayMinutes                  float64
     18  DepDel15                         float64
     19  TaxiOut                          float64
     20  WheelsOff                        float64
     21  WheelsOn                         float64
     22  TaxiIn                           float64
     23  CRSArrTime                       int64  
     24  ArrTime                          float64
     25  ArrDelay                         float64
     26  ArrDelayMinutes                  float64
     27  ArrDel15                         float64
     28  Cancelled                        float64
     29  CancellationCode                 str    
     30  Diverted                         float64
     31  CRSElapsedTime                   float64
     32  ActualElapsedTime                float64
     33  AirTime                          float64
     34  Distance                         float64
     35  CarrierDelay                     float64
     36  WeatherDelay                     float64
     37  NASDelay                         float64
     38  SecurityDelay                    float64
     39  LateAircraftDelay                float64
    dtypes: float64(23), int64(8), str(9)
    memory usage: 815.3 MB

Look at the missing values

``` python
flights.isna().sum().sort_values(ascending=False)
```

    CancellationCode                   2291851
    LateAircraftDelay                  1838744
    SecurityDelay                      1838744
    NASDelay                           1838744
    WeatherDelay                       1838744
    CarrierDelay                       1838744
    AirTime                              34019
    ActualElapsedTime                    34019
    ArrDel15                             34019
    ArrDelayMinutes                      34019
    ArrDelay                             34019
    ArrTime                              29738
    TaxiIn                               29738
    WheelsOn                             29738
    TaxiOut                              29099
    WheelsOff                            29099
    DepDelay                             28368
    DepDel15                             28368
    DepDelayMinutes                      28368
    DepTime                              28256
    Diverted                                 0
    Month                                    0
    DayofMonth                               0
    DayOfWeek                                0
    FlightDate                               0
    Distance                                 0
    Reporting_Airline                        0
    Flight_Number_Reporting_Airline          0
    CRSElapsedTime                           0
    Cancelled                                0
    Origin                                   0
    OriginCityName                           0
    OriginState                              0
    Dest                                     0
    DestCityName                             0
    CRSArrTime                               0
    DestState                                0
    CRSDepTime                               0
    Quarter                                  0
    Year                                     0
    dtype: int64

This looks reasonable - the reason for the high number of missing values
with Cancellation Code is that most flights in this data were delayed
rather than cancelled.

# Creating periods

Base period: 1 - 30 Sep Shutdown: 1 Oct - 6 Nov Sutdown & cuts: 7 - 12
Nov FAA cuts: 13 - 17 Nov Recovery/ Holidays: 18 Nov - 31 Dec

Some things to note: on Nov 15 the madatory flight cut was reduced from
6% to 3%. And the order was lifted Nov 17.

``` python
bins = pd.to_datetime(["2025-09-01", "2025-10-01", "2025-11-07",
                       "2025-11-13", "2025-11-18", "2026-01-01"]) # each day is the 1st day of the period, with the last period including 31 Dec
labels = ["Base", "Shutdown", "Shutdown & Cuts",
          "FAA Cuts", "Recovery/Holidays"]

flights["Period"] = pd.cut(flights["FlightDate"], bins=bins,
                           labels=labels, right=False) 
                           # right = FALSE: a period begins on its own edge and stops the day before the next

flights["Period"].value_counts(sort=False)
```

    Period
    Base                 562439
    Shutdown             717182
    Shutdown & Cuts      112913
    FAA Cuts              97418
    Recovery/Holidays    831185
    Name: count, dtype: int64

# Delayed Flights

# Cancelled Flights

# Diverted Flights
