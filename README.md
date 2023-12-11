# benchmark-datetime

Compare:

- [python-dateutil](https://github.com/dateutil/dateutil)
- [arrow](https://github.com/arrow-py/arrow/)
- [pendulum](https://github.com/sdispater/pendulum)
- [udatetime](https://github.com/freach/udatetime)
- Python [`datetime`](https://docs.python.org/3/library/datetime.html)
- [pydantic](https://github.com/pydantic/pydantic)

## How to setup

1. Install [Poetry](https://python-poetry.org/).
2. Activate the virtual environment and install dependencies: `poetry shell && poetry install`.

Some packages (for example, `udatetime`) may require additional dependencies for building from source.
Read the documentation for specific packages if the installation fails.

## How to run

```sh
pytest benchmark_datetime/benchmark.py --benchmark-group-by=func
```

## Results summary

If performance is important:

- Use Python `datetime` module for manipulating dates and times.
- Use Python `datetime` module, `udatetime` or `Pydantic` for parsing.

`pendulum` and `arrow` can be many times slower. Use third-party packages if you need additional functionality for manipulating dates, and a possible performance overhead can be ignored, and you are not going to implement these helper functions with a standard library.

## Results

```bash
------------------------------------------------------------------------------ benchmark 'test_add_timedelta': 4 tests -------------------------------------------------------------------------------
Name (time in us)                    Min                Max               Mean            StdDev             Median               IQR             Outliers  OPS (Kops/s)            Rounds  Iterations
------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
test_add_timedelta[python]        1.5440 (1.0)      32.7620 (1.0)       1.8408 (1.0)      0.8245 (1.0)       1.6780 (1.0)      0.0680 (1.0)      5250;6896      543.2527 (1.0)       78927           1
test_add_timedelta[pendulum]     10.3080 (6.68)     50.0460 (1.53)     11.2356 (6.10)     2.0123 (2.44)     10.8300 (6.45)     0.4970 (7.31)     2384;2684       89.0025 (0.16)      49520           1
test_add_timedelta[dateutil]     10.4040 (6.74)     60.0940 (1.83)     11.3538 (6.17)     2.1070 (2.56)     10.9420 (6.52)     0.3450 (5.07)     3117;4109       88.0764 (0.16)      63980           1
test_add_timedelta[arrow]        20.0120 (12.96)    66.6600 (2.03)     21.7338 (11.81)    3.5447 (4.30)     20.9180 (12.47)    0.7577 (11.14)    1742;2191       46.0112 (0.08)      24971           1
------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

--------------------------------------------------------------------------------------- benchmark 'test_convert_dt_to_isoformat_string': 4 tests ---------------------------------------------------------------------------------------
Name (time in ns)                                         Min                    Max                  Mean              StdDev                Median                IQR             Outliers  OPS (Kops/s)            Rounds  Iterations
----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
test_convert_dt_to_isoformat_string[udatetime]       537.5012 (1.0)       4,371.5991 (1.0)        580.1091 (1.0)      136.7538 (1.0)        558.2988 (1.0)      26.1993 (1.0)      4515;4689    1,723.8136 (1.0)      158104          10
test_convert_dt_to_isoformat_string[pendulum]      1,667.9987 (3.10)     22,109.0061 (5.06)     1,850.8343 (3.19)     436.1368 (3.19)     1,782.6678 (3.19)     71.6730 (2.74)     3653;5431      540.2969 (0.31)     139548           3
test_convert_dt_to_isoformat_string[python]        1,738.9830 (3.24)     24,011.9989 (5.49)     1,950.5207 (3.36)     686.7678 (5.02)     1,876.9933 (3.36)     80.0064 (3.05)     2180;5612      512.6836 (0.30)     172861           1
test_convert_dt_to_isoformat_string[arrow]         2,098.0078 (3.90)     29,330.0254 (6.71)     2,386.2115 (4.11)     832.9601 (6.09)     2,287.9976 (4.10)     91.9681 (3.51)     3265;7630      419.0743 (0.24)     188751           1
----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

-------------------------------------------------------------------------------------------- benchmark 'test_find_next_saturday': 4 tests -------------------------------------------------------------------------------------------
Name (time in ns)                             Min                     Max                   Mean                 StdDev                 Median                   IQR             Outliers  OPS (Kops/s)            Rounds  Iterations
-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
test_find_next_saturday[python]          628.2498 (1.0)        6,458.5038 (1.0)         715.2738 (1.0)         220.9866 (1.0)         687.5052 (1.0)         31.7450 (1.0)      3557;6145    1,398.0660 (1.0)      197239           4
test_find_next_saturday[dateutil]     10,757.0086 (17.12)     48,232.9924 (7.47)     11,992.9162 (16.77)     2,571.1358 (11.63)    11,369.0039 (16.54)      464.0024 (14.62)    3360;3906       83.3826 (0.06)      50772           1
test_find_next_saturday[arrow]        19,038.0088 (30.30)     72,082.9994 (11.16)    21,014.3948 (29.38)     4,074.5247 (18.44)    19,933.9993 (28.99)      881.9625 (27.78)    2846;3603       47.5864 (0.03)      37076           1
test_find_next_saturday[pendulum]     46,729.0229 (74.38)    538,406.9809 (83.36)    51,328.1108 (71.76)    10,193.9392 (46.13)    48,958.9875 (71.21)    2,073.0076 (65.30)    1067;2188       19.4825 (0.01)      16739           1
-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

----------------------------------------------------------------------------------- benchmark 'test_isoweekday': 4 tests ----------------------------------------------------------------------------------
Name (time in ns)                   Min                   Max                Mean             StdDev              Median               IQR             Outliers  OPS (Mops/s)            Rounds  Iterations
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
test_isoweekday[python]        120.5498 (1.0)        576.7401 (1.0)      129.4739 (1.0)      21.9267 (1.0)      125.3301 (1.0)      5.7800 (1.35)     3312;3647        7.7236 (1.0)       73535         100
test_isoweekday[udatetime]     122.0001 (1.01)     1,577.5952 (2.74)     132.5302 (1.02)     29.9487 (1.37)     128.1890 (1.02)     4.2712 (1.0)      5129;5804        7.5455 (0.98)     199244          37
test_isoweekday[pendulum]      219.3334 (1.82)     1,329.3812 (2.30)     237.7134 (1.84)     54.7252 (2.50)     230.2390 (1.84)     8.0964 (1.90)     4771;5552        4.2067 (0.54)     197006          21
test_isoweekday[arrow]         249.8426 (2.07)     1,846.0529 (3.20)     273.5535 (2.11)     68.6759 (3.13)     263.7895 (2.10)     9.6318 (2.26)     4966;5814        3.6556 (0.47)     185667          19
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

------------------------------------------------------------------------------------------- benchmark 'test_now_local': 4 tests -------------------------------------------------------------------------------------------
Name (time in ns)                     Min                     Max                   Mean                StdDev                 Median                   IQR            Outliers  OPS (Kops/s)            Rounds  Iterations
---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
test_now_local[udatetime]        175.2301 (1.0)          666.2399 (1.0)         186.6567 (1.0)         27.8917 (1.0)         181.1300 (1.0)          9.2003 (1.0)     2735;3348    5,357.4279 (1.0)       53229         100
test_now_local[python]           386.7291 (2.21)       3,221.6353 (4.84)        414.5507 (2.22)       105.6493 (3.79)        401.8181 (2.22)        18.3619 (2.00)    4070;4475    2,412.2501 (0.45)     200000          11
test_now_local[pendulum]       7,847.0039 (44.78)     43,125.9978 (64.73)     8,771.0899 (46.99)    1,966.3880 (70.50)     8,364.0043 (46.18)      348.9840 (37.93)   2967;3682      114.0109 (0.02)      60383           1
test_now_local[arrow]         48,752.0010 (278.22)   154,352.0193 (231.68)   53,016.8166 (284.03)   7,556.7886 (270.93)   50,898.0011 (281.00)   2,565.2553 (278.82)    631;935       18.8619 (0.00)       7929           1
---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

---------------------------------------------------------------------------------------- benchmark 'test_now_utc': 5 tests ----------------------------------------------------------------------------------------
Name (time in ns)                  Min                    Max                  Mean                StdDev                Median                 IQR            Outliers  OPS (Kops/s)            Rounds  Iterations
-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
test_now_utc[udatetime]       173.9702 (1.0)         575.1199 (1.0)        185.1021 (1.0)         28.7710 (1.0)        179.5898 (1.0)        5.4029 (1.0)     2957;3639    5,402.4226 (1.0)       54449         100
test_now_utc[python]          316.3994 (1.82)      2,538.0674 (4.41)       343.1992 (1.85)        89.1886 (3.10)       329.5989 (1.84)      15.3338 (2.84)    5041;5884    2,913.7595 (0.54)     184604          15
test_now_utc[dateutil]        475.6002 (2.73)      9,048.2994 (15.73)      525.4355 (2.84)       134.3873 (4.67)       505.0999 (2.81)      17.7999 (3.29)    4543;6123    1,903.1831 (0.35)     174096          10
test_now_utc[arrow]         3,438.0064 (19.76)    24,510.9841 (42.62)    3,782.6151 (20.44)    1,005.4565 (34.95)    3,649.9987 (20.32)    113.9706 (21.09)    657;1373      264.3674 (0.05)      35781           1
test_now_utc[pendulum]      3,627.0067 (20.85)    33,638.0035 (58.49)    4,065.0486 (21.96)    1,217.6759 (42.32)    3,868.0155 (21.54)    167.9873 (31.09)   3353;5912      245.9995 (0.05)     111446           1
-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

------------------------------------------------------------------------------------------- benchmark 'test_parse_utc_from_iso_8601': 5 tests --------------------------------------------------------------------------------------------
Name (time in ns)                                   Min                     Max                   Mean                 StdDev                 Median                   IQR            Outliers  OPS (Kops/s)            Rounds  Iterations
------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
test_parse_utc_from_iso_8601[python]           138.4001 (1.0)          626.0900 (1.0)         148.2330 (1.0)          24.1436 (1.0)         143.7397 (1.0)          6.4800 (1.0)     3008;3513    6,746.1347 (1.0)       64575         100
test_parse_utc_from_iso_8601[pydantic]         890.9847 (6.44)      21,868.9966 (34.93)     1,029.7347 (6.95)        535.2250 (22.17)       982.9819 (6.84)        50.0004 (7.72)    1258;4072      971.1239 (0.14)     108531           1
test_parse_utc_from_iso_8601[udatetime]        962.9992 (6.96)       8,668.2034 (13.84)     1,036.0871 (6.99)        253.1262 (10.48)     1,001.6025 (6.97)        45.1982 (6.98)    4026;5017      965.1698 (0.14)     173371           5
test_parse_utc_from_iso_8601[pendulum]      20,239.9970 (146.24)    76,755.9977 (122.60)   22,261.1677 (150.18)    3,904.0466 (161.70)   21,279.9932 (148.05)     837.9866 (129.32)   848;1027       44.9213 (0.01)      11478           1
test_parse_utc_from_iso_8601[arrow]         73,036.0043 (527.72)   188,874.9830 (301.67)   79,179.3078 (534.15)   10,276.3488 (425.64)   76,067.9832 (529.21)   3,843.7574 (593.18)    594;995       12.6296 (0.00)       7847           1
------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

--------------------------------------------------------------------------------------------- benchmark 'test_parse_utc_from_rfc_3339': 5 tests ----------------------------------------------------------------------------------------------
Name (time in ns)                                    Min                     Max                    Mean                 StdDev                  Median                    IQR            Outliers  OPS (Kops/s)            Rounds  Iterations
----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
test_parse_utc_from_rfc_3339[python]            548.2987 (1.0)       11,953.1003 (1.0)          589.7065 (1.0)         132.9931 (1.0)          571.5992 (1.0)          21.6009 (1.0)     4003;4499    1,695.7588 (1.0)      156888          10
test_parse_utc_from_rfc_3339[pydantic]        2,047.9783 (3.74)      30,013.9945 (2.51)       2,341.0325 (3.97)        891.8920 (6.71)       2,243.0031 (3.92)         94.0345 (4.35)    1505;3541      427.1619 (0.25)     102187           1
test_parse_utc_from_rfc_3339[udatetime]       3,032.0080 (5.53)      26,191.0027 (2.19)       3,369.1546 (5.71)        937.8039 (7.05)       3,255.0015 (5.69)        116.9974 (5.42)    2449;4689      296.8104 (0.18)     146714           1
test_parse_utc_from_rfc_3339[pendulum]       54,978.0088 (100.27)   171,351.0137 (14.34)     59,585.4149 (101.04)    7,986.3676 (60.05)     57,328.9872 (100.30)    2,752.2256 (127.41)   926;1514       16.7826 (0.01)      11541           1
test_parse_utc_from_rfc_3339[arrow]         210,628.0008 (384.15)   481,992.9891 (40.32)    228,545.8559 (387.56)   27,347.3763 (205.63)   219,459.0052 (383.94)   13,973.0109 (646.87)    108;141        4.3755 (0.00)       1206           1
----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

----------------------------------------------------------------------------------------- benchmark 'test_parse_utc_from_timestamp': 5 tests ----------------------------------------------------------------------------------------
Name (time in ns)                                   Min                    Max                  Mean                StdDev                Median                 IQR             Outliers  OPS (Kops/s)            Rounds  Iterations
-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
test_parse_utc_from_timestamp[python]          164.7501 (1.0)         988.3801 (1.0)        175.2152 (1.0)         26.6586 (1.0)        170.4801 (1.0)        4.9302 (1.0)      2787;3543    5,
707.2669 (1.0)       58521         100
test_parse_utc_from_timestamp[udatetime]       195.0007 (1.18)      1,546.3915 (1.56)       210.8802 (1.20)        57.2576 (2.15)       203.3472 (1.19)       8.9121 (1.81)     1677;1859    4,742.0287 (0.83)      70024          23
test_parse_utc_from_timestamp[pydantic]      1,922.0170 (11.67)    50,886.0103 (51.48)    2,148.0009 (12.26)      800.5752 (30.03)    2,057.9901 (12.07)     77.9692 (15.81)    2792;6734      465.5491 (0.08)     178923           1
test_parse_utc_from_timestamp[pendulum]      4,263.0127 (25.88)    35,886.0125 (36.31)    4,746.6475 (27.09)    1,446.0470 (54.24)    4,510.9773 (26.46)    183.0340 (37.13)    3876;5534      210.6750 (0.04)     114745           1
test_parse_utc_from_timestamp[arrow]         5,732.9889 (34.80)    46,132.0160 (46.67)    6,355.0551 (36.27)    1,630.9126 (61.18)    6,063.9868 (35.57)    255.0078 (51.72)    2695;3481      157.3550 (0.03)      69185           1
-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

------------------------------------------------------------------------------- benchmark 'test_substract_timedelta': 4 tests -------------------------------------------------------------------------------
Name (time in us)                          Min                Max               Mean            StdDev             Median               IQR              Outliers  OPS (Kops/s)            Rounds  Iterations
-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
test_substract_timedelta[python]        1.4323 (1.0)      11.1153 (1.0)       1.6285 (1.0)      0.4995 (1.0)       1.5213 (1.0)      0.0547 (1.0)      9026;10816      614.0721 (1.0)      167505           3
test_substract_timedelta[pendulum]     10.5990 (7.40)     47.9960 (4.32)     11.9269 (7.32)     2.7153 (5.44)     11.2110 (7.37)     0.3750 (6.86)      4250;5448       83.8439 (0.14)      57871           1
test_substract_timedelta[dateutil]     10.6790 (7.46)     53.0950 (4.78)     11.7521 (7.22)     2.3798 (4.76)     11.2260 (7.38)     0.4750 (8.69)      3453;4119       85.0909 (0.14)      58313           1
test_substract_timedelta[arrow]        20.2610 (14.15)    98.4780 (8.86)     22.4082 (13.76)    4.1118 (8.23)     21.2240 (13.95)    0.6880 (12.59)     2670;3440       44.6265 (0.07)      33005           1
-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

----------------------------------------------------------------------------------- benchmark 'test_timedelta_to_seconds': 2 tests ----------------------------------------------------------------------------------
Name (time in ns)                            Min                   Max                Mean             StdDev              Median                IQR             Outliers  OPS (Mops/s)            Rounds  Iterations
---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
test_timedelta_to_seconds[python]       253.4729 (1.0)      1,974.1588 (1.0)      272.9762 (1.0)      64.8166 (1.0)      263.5260 (1.0)       9.2121 (1.0)      5985;7088        3.6633 (1.0)      192124          19
test_timedelta_to_seconds[pendulum]     270.8830 (1.07)     2,056.9406 (1.04)     294.5300 (1.08)     70.8906 (1.09)     284.7074 (1.08)     10.8814 (1.18)     5614;6445        3.3952 (0.93)     194553          17
---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

Legend:
  Outliers: 1 Standard Deviation from Mean; 1.5 IQR (InterQuartile Range) from 1st Quartile and 3rd Quartile.
  OPS: Operations Per Second, computed as 1 / Mean
```
