# benchmark-datetime

Compare:

- Python 3.12.3 [`datetime`](https://docs.python.org/3/library/datetime.html)
- [python-dateutil](https://github.com/dateutil/dateutil)
- [arrow](https://github.com/arrow-py/arrow/)
- [pendulum](https://github.com/sdispater/pendulum)
- [udatetime](https://github.com/freach/udatetime)
- [pydantic](https://github.com/pydantic/pydantic) (used only in benchmarks where we parse an argument)
- [pandas](https://pandas.pydata.org/) (used only in benchmarks where we parse an argument)

There are three types of benchmarks or three types of actions you can do in your code:

- Manipulating datetime, date, duration objects.
- Parsing (for example, converting a string to an object).
- Dumping (for example, converting an object to a string).

## How to setup

1. Install [Poetry](https://python-poetry.org/).
2. Activate the virtual environment and install dependencies: `poetry shell && poetry install`.

Some packages (for example, `udatetime`) may require additional dependencies for building from source.
Read the documentation for specific packages if the installation fails.

Suggestions and contributions are welcome.

## How to run

```sh
pytest benchmark/ --benchmark-group-by=func --benchmark-histogram
```

## Histogram

### Parsing

![](./benchmark_histogram/parse/benchmark-test_parse_utc_from_iso_8601.svg)
![](./benchmark_histogram/parse/benchmark-test_parse_utc_from_rfc_3339.svg)
![](./benchmark_histogram/parse/benchmark-test_parse_utc_from_iso_8601_duration.svg)
![](./benchmark_histogram/parse/benchmark-test_parse_utc_from_timestamp.svg)

### Manipulating

![](./benchmark_histogram/manipulate/benchmark-test_now_utc.svg)
![](./benchmark_histogram/manipulate/benchmark-test_now_local.svg)
![](./benchmark_histogram/manipulate/benchmark-test_isoweekday.svg)
![](./benchmark_histogram/manipulate/benchmark-test_add_timedelta.svg)
![](./benchmark_histogram/manipulate/benchmark-test_find_next_saturday.svg)
![](./benchmark_histogram/manipulate/benchmark-test_timedelta_to_seconds.svg)

### Dumping

![](./benchmark_histogram/dump/benchmark-test_convert_dt_to_isoformat_string.svg)

## Results summary

If performance is important:

- Use Python `datetime` module for manipulating dates and times.
- Use Python `datetime` module, `udatetime` or `Pydantic` for parsing.

`pendulum` and `arrow` can be several times slower. Use third-party packages if you need additional functionality for manipulating dates, and a possible performance overhead can be ignored, and you are not going to implement these helper functions with a standard library.

## Results

```bash

------------------------------------------------------------------------------ benchmark 'test_add_timedelta': 4 tests ------------------------------------------------------------------------------
Name (time in us)                    Min                Max               Mean            StdDev             Median               IQR            Outliers  OPS (Kops/s)            Rounds  Iterations
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
test_add_timedelta[python]        1.3777 (1.0)       9.4570 (1.0)       1.4470 (1.0)      0.1682 (1.0)       1.4270 (1.0)      0.0237 (1.0)    2329;10254      691.0744 (1.0)      158857           3
test_add_timedelta[dateutil]     10.3410 (7.51)     34.2670 (3.62)     10.7714 (7.44)     0.7853 (4.67)     10.6600 (7.47)     0.1270 (5.37)    1532;4491       92.8383 (0.13)      67486           1
test_add_timedelta[pendulum]     13.0140 (9.45)     40.3800 (4.27)     13.4547 (9.30)     0.7906 (4.70)     13.3440 (9.35)     0.1190 (5.03)    1076;2209       74.3232 (0.11)      43195           1
test_add_timedelta[arrow]        20.2350 (14.69)    61.2210 (6.47)     20.8205 (14.39)    1.0446 (6.21)     20.6510 (14.47)    0.1610 (6.80)     713;1436       48.0295 (0.07)      22378           1
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------


--------------------------------------------------------------------------------------- benchmark 'test_convert_dt_to_isoformat_string': 4 tests --------------------------------------------------------------------------------------
Name (time in ns)                                         Min                    Max                  Mean              StdDev                Median                IQR            Outliers  OPS (Kops/s)            Rounds  Iterations
---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
test_convert_dt_to_isoformat_string[udatetime]       771.7004 (1.0)       2,948.6997 (1.0)        780.8776 (1.0)       47.8781 (1.0)        776.2006 (1.0)       2.6004 (1.0)     1140;6161    1,280.6105 (1.0)      118274          10
test_convert_dt_to_isoformat_string[python]        2,003.5004 (2.60)     11,988.5008 (4.07)     2,096.7291 (2.69)     207.6502 (4.34)     2,074.0008 (2.67)     35.4958 (13.65)  2831;13091      476.9333 (0.37)     183858           2
test_convert_dt_to_isoformat_string[arrow]         2,556.9971 (3.31)     18,687.9988 (6.34)     2,709.2313 (3.47)     507.6455 (10.60)    2,667.9991 (3.44)     65.0034 (25.00)    370;1380      369.1084 (0.29)      50343           1
test_convert_dt_to_isoformat_string[pendulum]      2,954.9992 (3.83)     22,440.9960 (7.61)     3,099.2284 (3.97)     320.3608 (6.69)     3,069.9957 (3.96)     54.9990 (21.15)   2452;7038      322.6610 (0.25)     163667           1
---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------


----------------------------------------------------------------------------------------- benchmark 'test_find_next_saturday': 4 tests -----------------------------------------------------------------------------------------
Name (time in ns)                             Min                    Max                   Mean                StdDev                 Median                 IQR            Outliers  OPS (Kops/s)            Rounds  Iterations
--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
test_find_next_saturday[python]          562.6003 (1.0)       2,850.2000 (1.0)         587.7329 (1.0)         53.5427 (1.0)         584.3998 (1.0)        9.8000 (1.0)      918;3641    1,701.4532 (1.0)      108543          10
test_find_next_saturday[dateutil]     10,782.0015 (19.16)    52,891.0023 (18.56)    11,282.0677 (19.20)      902.4286 (16.85)    11,160.0020 (19.10)    144.0058 (14.69)   1528;3446       88.6362 (0.05)      64272           1
test_find_next_saturday[arrow]        19,154.9989 (34.05)    52,919.0002 (18.57)    19,734.1844 (33.58)    1,070.0743 (19.99)    19,567.9968 (33.48)    149.0043 (15.20)   1173;2166       50.6735 (0.03)      36935           1
test_find_next_saturday[pendulum]     32,342.0027 (57.49)    69,496.0036 (24.38)    33,126.3802 (56.36)    1,186.5940 (22.16)    32,916.0030 (56.32)    223.0008 (22.76)   1053;1740       30.1874 (0.02)      24387           1
--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------


----------------------------------------------------------------------------------------- benchmark 'test_now_local': 4 tests ------------------------------------------------------------------------------------------
Name (time in ns)                     Min                    Max                   Mean                StdDev                 Median                 IQR            Outliers  OPS (Kops/s)            Rounds  Iterations
------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
test_now_local[udatetime]        187.2919 (1.0)         970.2083 (1.0)         191.2808 (1.0)         16.3155 (1.0)         190.1250 (1.0)        1.7499 (1.10)    1211;9269    5,227.9151 (1.0)      198334          24
test_now_local[python]           411.6664 (2.20)      1,906.0832 (1.96)        417.9722 (2.19)        26.2972 (1.61)        416.0835 (2.19)       1.5837 (1.0)     1104;7198    2,392.5035 (0.46)     187512          12
test_now_local[pendulum]       2,279.0045 (12.17)    24,706.9984 (25.47)     2,474.8136 (12.94)      523.8641 (32.11)     2,423.9998 (12.75)     63.9993 (40.41)   2422;7815      404.0708 (0.08)     184468           1
test_now_local[arrow]         48,475.9948 (258.83)   94,399.0017 (97.30)    49,960.3951 (261.19)   1,568.3737 (96.13)    49,640.9994 (261.10)   479.2528 (302.61)   832;1035       20.0159 (0.00)      13461           1
------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------


--------------------------------------------------------------------------------------- benchmark 'test_now_utc': 5 tests ---------------------------------------------------------------------------------------
Name (time in ns)                  Min                    Max                  Mean              StdDev                Median                IQR             Outliers  OPS (Kops/s)            Rounds  Iterations
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
test_now_utc[udatetime]       181.8400 (1.0)         865.0500 (1.0)        184.4154 (1.0)        9.0506 (1.0)        183.2900 (1.0)       0.6000 (1.0)      1171;3941    5,422.5413 (1.0)       51144         100
test_now_utc[python]          329.2854 (1.81)      2,357.6431 (2.73)       335.7876 (1.82)      25.2201 (2.79)       333.8573 (1.82)      2.1428 (3.57)     1162;7774    2,978.0735 (0.55)     193051          14
test_now_utc[dateutil]        512.4995 (2.82)      3,624.7999 (4.19)       527.4641 (2.86)      38.4500 (4.25)       524.1003 (2.86)      3.9996 (6.67)     1248;7646    1,895.8635 (0.35)     171058          10
test_now_utc[pendulum]      2,212.0003 (12.16)    36,348.0067 (42.02)    2,380.1713 (12.91)    453.4516 (50.10)    2,330.0017 (12.71)    63.0025 (105.01)   2641;7713      420.1378 (0.08)     179373           1
test_now_utc[arrow]         3,455.0030 (19.00)    24,911.0017 (28.80)    3,707.0545 (20.10)    531.9252 (58.77)    3,649.0019 (19.91)    78.9951 (131.66)   1624;3914      269.7559 (0.05)     106079           1
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------


------------------------------------------------------------------------------------------ benchmark 'test_parse_utc_from_iso_8601': 7 tests ------------------------------------------------------------------------------------------
Name (time in ns)                                   Min                     Max                   Mean                StdDev                 Median                 IQR            Outliers  OPS (Kops/s)            Rounds  Iterations
---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
test_parse_utc_from_iso_8601[python]           136.6000 (1.0)          358.9700 (1.0)         140.5325 (1.0)          5.8714 (1.0)         138.9400 (1.0)        4.4100 (1.0)     1125;1060    7,115.7914 (1.0)       68564         100
test_parse_utc_from_iso_8601[pydantic]         536.2499 (3.93)       3,557.1256 (9.91)        568.0249 (4.04)        54.9055 (9.35)        563.1246 (4.05)      12.2500 (2.78)    1546;7667    1,760.4862 (0.25)     185840           8
test_parse_utc_from_iso_8601[udatetime]        965.5996 (7.07)       5,729.1996 (15.96)       987.9847 (7.03)        88.4277 (15.06)       979.6000 (7.05)       6.5993 (1.50)   1469;15410    1,012.1615 (0.14)     185564           5
test_parse_utc_from_iso_8601[pandas]         5,345.0058 (39.13)     25,542.0018 (71.15)     5,637.5056 (40.12)      474.0446 (80.74)     5,581.9983 (40.18)    102.9985 (23.36)   1666;3212      177.3834 (0.02)      91017           1
test_parse_utc_from_iso_8601[dateutil]      11,748.9981 (86.01)     35,810.9974 (99.76)    12,203.1457 (86.84)      646.8194 (110.16)   12,119.0023 (87.22)    134.0013 (30.39)   1037;1910       81.9461 (0.01)      45202           1
test_parse_utc_from_iso_8601[pendulum]      12,517.9940 (91.64)     46,045.9996 (128.27)   13,071.3159 (93.01)    1,183.2546 (201.53)   12,901.0004 (92.85)    197.0038 (44.67)    947;2421       76.5034 (0.01)      36339           1
test_parse_utc_from_iso_8601[arrow]         74,172.9964 (542.99)   140,215.0010 (390.60)   75,776.3286 (539.21)   2,592.7289 (441.59)   75,152.0056 (540.90)   463.0056 (104.99)    730;903       13.1967 (0.00)       8238           1
---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------


----------------------------------------------------------------------------------------- benchmark 'test_parse_utc_from_iso_8601_duration': 3 tests -----------------------------------------------------------------------------------------
Name (time in ns)                                           Min                    Max                   Mean                StdDev                 Median                 IQR            Outliers  OPS (Kops/s)            Rounds  Iterations
----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
test_parse_utc_from_iso_8601_duration[pydantic]        430.4500 (1.0)       3,700.9999 (1.0)         454.9460 (1.0)         34.9101 (1.0)         451.5499 (1.0)       11.0002 (1.0)     1233;2111    2,198.0631 (1.0)      104833          20
test_parse_utc_from_iso_8601_duration[pendulum]     10,380.0048 (24.11)    37,485.0060 (10.13)    10,844.6150 (23.84)      679.6950 (19.47)    10,749.9945 (23.81)    133.0045 (12.09)    943;2392       92.2117 (0.04)      40463           1
test_parse_utc_from_iso_8601_duration[pandas]       12,851.0001 (29.85)    63,044.0045 (17.03)    13,738.5289 (30.20)    2,129.0166 (60.99)    13,286.0005 (29.42)    252.0028 (22.91)   2132;4507       72.7880 (0.03)      49814           1
----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------


--------------------------------------------------------------------------------------------- benchmark 'test_parse_utc_from_rfc_3339': 6 tests ---------------------------------------------------------------------------------------------
Name (time in ns)                                    Min                     Max                    Mean                 StdDev                  Median                   IQR            Outliers  OPS (Kops/s)            Rounds  Iterations
---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
test_parse_utc_from_rfc_3339[python]            546.8995 (1.0)        2,936.7999 (1.0)          561.9617 (1.0)          48.7457 (1.0)          557.4002 (1.0)          4.7999 (1.0)     1330;9474    1,779.4808 (1.0)      161996          10
test_parse_utc_from_rfc_3339[pydantic]        1,888.0019 (3.45)      45,486.0055 (15.49)      2,061.8166 (3.67)        343.7071 (7.05)       2,041.0007 (3.66)        72.0029 (15.00)    781;1966      485.0092 (0.27)      84589           1
test_parse_utc_from_rfc_3339[udatetime]       2,963.9996 (5.42)      23,734.9959 (8.08)       3,111.6239 (5.54)        342.1367 (7.02)       3,083.0051 (5.53)        56.9999 (11.88)   1121;4460      321.3756 (0.18)     135300           1
test_parse_utc_from_rfc_3339[pandas]         36,867.9976 (67.41)     52,169.9985 (17.76)     37,897.3441 (67.44)     2,257.4381 (46.31)     37,352.0015 (67.01)      346.9904 (72.29)        7;12       26.3871 (0.01)        122           1
test_parse_utc_from_rfc_3339[pendulum]       37,949.0011 (69.39)    122,067.9987 (41.56)     40,770.5634 (72.55)     7,038.6840 (144.40)    38,787.9991 (69.59)    1,089.0071 (226.88)   856;1418       24.5275 (0.01)      13577           1
test_parse_utc_from_rfc_3339[arrow]         212,758.0010 (389.03)   429,957.9959 (146.40)   227,309.2798 (404.49)   32,289.9198 (662.42)   219,326.0007 (393.48)   7,462.2531 (>1000.0)    81;166        4.3993 (0.00)       1297           1
---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------


--------------------------------------------------------------------------------------- benchmark 'test_parse_utc_from_timestamp': 5 tests ---------------------------------------------------------------------------------------
Name (time in ns)                                   Min                    Max                  Mean              StdDev                Median                 IQR            Outliers  OPS (Kops/s)            Rounds  Iterations
----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
test_parse_utc_from_timestamp[python]          164.2699 (1.0)         750.0300 (1.0)        166.6250 (1.0)        6.1398 (1.0)        166.0699 (1.0)        0.5600 (1.0)     1091;2015    6,001.4999 (1.0)       59355         100
test_parse_utc_from_timestamp[udatetime]       319.9966 (1.95)     27,966.0017 (37.29)      362.5104 (2.18)     208.5176 (33.96)      354.0044 (2.13)      11.9981 (21.43)    399;7142    2,758.5419 (0.46)     139978           1
test_parse_utc_from_timestamp[pydantic]      1,952.0012 (11.88)    23,296.0010 (31.06)    2,076.8808 (12.46)    349.0958 (56.86)    2,052.9988 (12.36)     47.0027 (83.94)   1538;7027      481.4913 (0.08)     195695           1
test_parse_utc_from_timestamp[arrow]         5,783.0039 (35.20)    29,353.9997 (39.14)    6,076.7740 (36.47)    534.9664 (87.13)    6,016.9987 (36.23)     99.9935 (178.57)  1310;2604      164.5610 (0.03)      76197           1
test_parse_utc_from_timestamp[pendulum]      9,022.9951 (54.93)    40,260.0053 (53.68)    9,399.1899 (56.41)    727.1538 (118.43)   9,303.9998 (56.02)    107.0002 (191.09)  1354;3889      106.3921 (0.02)      66761           1
----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------


------------------------------------------------------------------------------ benchmark 'test_substract_timedelta': 4 tests -------------------------------------------------------------------------------
Name (time in us)                          Min                Max               Mean            StdDev             Median               IQR             Outliers  OPS (Kops/s)            Rounds  Iterations
------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
test_substract_timedelta[python]        1.4470 (1.0)      10.2197 (1.0)       1.5154 (1.0)      0.1647 (1.0)       1.4970 (1.0)      0.0220 (1.0)      2653;7934      659.8835 (1.0)      179534           3
test_substract_timedelta[dateutil]     10.5660 (7.30)     46.7380 (4.57)     11.1257 (7.34)     1.2266 (7.45)     10.9330 (7.30)     0.1350 (6.14)     1768;4846       89.8817 (0.14)      59256           1
test_substract_timedelta[pendulum]     13.2590 (9.16)     40.6270 (3.98)     13.6792 (9.03)     0.7664 (4.65)     13.5700 (9.06)     0.1230 (5.59)     1293;2895       73.1037 (0.11)      52660           1
test_substract_timedelta[arrow]        20.3030 (14.03)    48.7710 (4.77)     20.8930 (13.79)    1.0844 (6.58)     20.7180 (13.84)    0.1830 (8.32)     1041;2143       47.8630 (0.07)      32341           1
------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------


----------------------------------------------------------------------------------- benchmark 'test_timedelta_to_seconds': 2 tests ----------------------------------------------------------------------------------
Name (time in ns)                            Min                   Max                Mean             StdDev              Median               IQR              Outliers  OPS (Mops/s)            Rounds  Iterations
---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
test_timedelta_to_seconds[python]       251.2222 (1.0)      1,612.3886 (1.0)      258.5957 (1.0)      21.6611 (1.0)      255.4443 (1.0)      2.0559 (1.0)      3084;25777        3.8670 (1.0)      199243          18
test_timedelta_to_seconds[pendulum]     267.6470 (1.07)     1,661.2355 (1.03)     281.8234 (1.09)     28.2951 (1.31)     279.4118 (1.09)     8.5878 (4.18)      2857;3330        3.5483 (0.92)     196657          17
---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------


---------------------------------------------------------------------------------- benchmark 'test_weekday': 4 tests ----------------------------------------------------------------------------------
Name (time in ns)                Min                   Max                Mean             StdDev              Median               IQR            Outliers  OPS (Mops/s)            Rounds  Iterations
-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
test_weekday[python]        117.1700 (1.0)        412.5700 (1.09)     119.4933 (1.0)       7.5511 (1.0)      118.7300 (1.0)      0.6000 (1.0)     1166;4945        8.3687 (1.0)       78297         100
test_weekday[udatetime]     117.3301 (1.00)       377.2100 (1.0)      120.7656 (1.01)     13.9809 (1.85)     118.8400 (1.00)     0.6099 (1.02)    1952;5586        8.2805 (0.99)      80743         100
test_weekday[arrow]         243.1054 (2.07)     1,256.6843 (3.33)     251.2140 (2.10)     19.8574 (2.63)     249.9471 (2.11)     2.3153 (3.86)   1137;12618        3.9807 (0.48)     191351          19
test_weekday[pendulum]      684.1998 (5.84)     2,870.7997 (7.61)     702.6246 (5.88)     42.0854 (5.57)     698.9998 (5.89)     5.9001 (9.83)     879;3959        1.4232 (0.17)     101989          10
-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------


Legend:
  Outliers: 1 Standard Deviation from Mean; 1.5 IQR (InterQuartile Range) from 1st Quartile and 3rd Quartile.
  OPS: Operations Per Second, computed as 1 / Mean
```
