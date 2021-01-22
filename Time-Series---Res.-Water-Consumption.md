Time Series - Water Consumption
================
Bill Peterson
12/5/2019

-   [The Data - Residential Water Consumption in London, 1983 - 1994](#the-data---residential-water-consumption-in-london-1983---1994)
-   [Graphs of the relationships between X and Y; and expectations of X and Y relationship](#graphs-of-the-relationships-between-x-and-y-and-expectations-of-x-and-y-relationship)
-   [Model 1: A simple time series regression, with one X and no trend](#model-1-a-simple-time-series-regression-with-one-x-and-no-trend)
-   [Model 2: A time series regression with one X and trend](#model-2-a-time-series-regression-with-one-x-and-trend)
-   [Model 3: A time series regression with many Xs and trend; autocorrelation diagnostics](#model-3-a-time-series-regression-with-many-xs-and-trend-autocorrelation-diagnostics)
    -   [Water consumption = number of consumers + precipitation in ml + temperature in celsius + month + time in months + error](#water-consumption-number-of-consumers-precipitation-in-ml-temperature-in-celsius-month-time-in-months-error)
        -   [BP test](#bp-test)
        -   [VIF interpretation](#vif-interpretation)
        -   [ACF plot of the residuals for Model 3](#acf-plot-of-the-residuals-for-model-3)
        -   [Durbin-Watson and Breusch-Godfrey tests](#durbin-watson-and-breusch-godfrey-tests)
-   [First differenced time series regression](#first-differenced-time-series-regression)
    -   [First differenced model interpretation](#first-differenced-model-interpretation)
    -   [Durbin-Watson and Breusch-Godfrey tests - First differenced Model 3](#durbin-watson-and-breusch-godfrey-tests---first-differenced-model-3)
-   [Check for unit roots.](#check-for-unit-roots.)
-   [Auto ARIMA on the residuals of Model 2](#auto-arima-on-the-residuals-of-model-2)
-   [Auto ARIMA on the residuals of Model 3](#auto-arima-on-the-residuals-of-model-3)

## The Data - Residential Water Consumption in London, 1983 - 1994

The dataset was obtained from the R package "**tsdl**" via GitHub, and is comprised of monthly observations from January 1983 through April 1994. <https://github.com/FinYang/tsdl/tree/master/data-raw/londonwq>

The relevant tsdl data sets we want:

\[\[249\]\] "Total number of water consumers, Jan 1983 – April 1994. Missing value for June 1988 (66th obs.) estimated by intervention analysis. London, United Kingdom."

\[\[344\]\] "Monthly precipitation (in mm), Jan 1983 – April 1994. London, United Kingdom."

\[\[378\]\] "Monthly temperature (in Celsius), Jan 1983 – April 1994. London, United Kingdom."

\[\[393\]\] "Residential water consumption, Jan 1983 – April 1994. Missing value for June 1988 (66th obs.) estimated by intervention analysis. London, United Kingdom."

![](Time-Series---Res.-Water-Consumption_files/figure-markdown_github/unnamed-chunk-2-1.png)

The faceted series plots of the variables over time clearly show seasonality in consumption, precipitation, and temperature. (Note: the water\_consump consumption variable was scaled by 1 million (consump\_1M) for the series plot.) The number of consumers shows also positive drift overtime. The seasonality and drift all seem somehwat intuitive for these variables.

<!--html_preserve-->

<script type="application/json" data-for="htmlwidget-dff0539911ccd7a8e3b2">{"x":{"html":"<table class=\"table table-condensed\">\n <thead>\n  <tr>\n   <th style=\"text-align:right;\"> Variable <\/th>\n   <th style=\"text-align:right;\"> Minimum <\/th>\n   <th style=\"text-align:right;\"> Quartile.1st <\/th>\n   <th style=\"text-align:right;\"> Median <\/th>\n   <th style=\"text-align:right;\"> Mean <\/th>\n   <th style=\"text-align:right;\"> Quartile.3rd <\/th>\n   <th style=\"text-align:right;\"> Maximum <\/th>\n   <th style=\"text-align:right;\"> Sparkline <\/th>\n  <\/tr>\n <\/thead>\n<tbody>\n  <tr>\n   <td style=\"text-align:right;\"> water_consump <\/td>\n   <td style=\"text-align:right;\"> 31802013 <\/td>\n   <td style=\"text-align:right;\"> 49528086 <\/td>\n   <td style=\"text-align:right;\"> 55381808 <\/td>\n   <td style=\"text-align:right;\"> 57265558 <\/td>\n   <td style=\"text-align:right;\"> 63764154 <\/td>\n   <td style=\"text-align:right;\"> 105205178 <\/td>\n   <td style=\"text-align:right;\"> <span id=\"htmlwidget-027a67527602be6152b7\" class=\"sparkline html-widget\"><\/span>\n<script type=\"application/json\" data-for=\"htmlwidget-027a67527602be6152b7\">{\"x\":{\"values\":[39380250,43352569,48804249,38577311,51736944,52592569,49675760,72247376,68211427,64519626,60172503,44675551,45603722,43944919,51504681,37701628,50134461,57587275,51753941,63703945,55371629,51014093,68991433,31802013,44676726,51618995,45157664,44669689,56233606,52091230,72125308,71545973,62253865,59311339,56953858,40357964,53587755,50620499,45875110,50446548,55391987,51516196,70189242,58792913,59448721,59808175,61217663,41021367,61416921,39484107,56123187,43833755,53319837,79312266,86574777,80261369,78863607,64434798,60472726,47585042,54208204,58403597,56475322,47668697,52348045,69323880,100582658,105205178,73405903,61761980,59935159,45708957,65553063,51251343,58300163,47891860,50954604,62076389,70359185,97653444,78878241,76009331,61751053,42508235,60534297,53488702,56621147,52218845,62804935,61454213,70416701,76855594,59221683,64519626,60172503,43375551,47525506,54778860,44317601,53260373,53677209,59023305,73000172,75435772,74390116,64866052,50131642,49599322,49640020,49314376,55999172,44696022,55942755,66369104,64497024,55969723,63944781,50290335,50956222,48400480,47660472,43927473,50841346,45565837,51743702,58903973,64189897,64024299,74033281,52833385,51632411,47467431,44346357,53317327,55675090,46374745],\"options\":{\"type\":\"line\",\"height\":20,\"width\":60},\"width\":60,\"height\":20},\"evals\":[],\"jsHooks\":[]}<\/script> <\/td>\n  <\/tr>\n  <tr>\n   <td style=\"text-align:right;\"> num_consumers <\/td>\n   <td style=\"text-align:right;\"> 23885 <\/td>\n   <td style=\"text-align:right;\"> 29133 <\/td>\n   <td style=\"text-align:right;\"> 31252 <\/td>\n   <td style=\"text-align:right;\"> 31389 <\/td>\n   <td style=\"text-align:right;\"> 33100 <\/td>\n   <td style=\"text-align:right;\"> 39654 <\/td>\n   <td style=\"text-align:right;\"> <span id=\"htmlwidget-9a62a597b6ae173ba3d2\" class=\"sparkline html-widget\"><\/span>\n<script type=\"application/json\" data-for=\"htmlwidget-9a62a597b6ae173ba3d2\">{\"x\":{\"values\":[24963,27380,32588,25511,32313,31858,23929,30452,28688,24936,34078,25601,27238,28126,32845,26001,29377,33275,28720,31159,23885,29233,39031,25918,27885,28695,30102,30110,32692,28224,30919,30229,28395,32274,32992,26956,28996,28154,27478,32720,30466,27618,29949,29821,28098,32021,29145,29428,28863,28687,33043,29092,28731,34249,31617,30542,30420,32422,33084,29167,30164,32655,34654,29570,29096,33523,30801,35274,31433,32584,32625,27569,36909,30822,36301,32327,33149,33027,29178,35109,29988,38742,38934,28986,35780,31676,35069,33238,36737,32407,33399,37719,30027,37644,39654,28707,28942,31080,26923,32836,32000,30802,31344,32409,33971,33584,30248,30469,30505,31112,31784,31682,30539,36502,32139,30369,36918,32316,33333,30016,30559,31478,35299,30442,32145,35360,34610,33873,37132,32695,34048,32256,27517,33454,38539,31775],\"options\":{\"type\":\"line\",\"height\":20,\"width\":60},\"width\":60,\"height\":20},\"evals\":[],\"jsHooks\":[]}<\/script> <\/td>\n  <\/tr>\n  <tr>\n   <td style=\"text-align:right;\"> precipitation_ml <\/td>\n   <td style=\"text-align:right;\"> 9.60 <\/td>\n   <td style=\"text-align:right;\"> 60.85 <\/td>\n   <td style=\"text-align:right;\"> 80.25 <\/td>\n   <td style=\"text-align:right;\"> 85.66 <\/td>\n   <td style=\"text-align:right;\"> 108.08 <\/td>\n   <td style=\"text-align:right;\"> 236.00 <\/td>\n   <td style=\"text-align:right;\"> <span id=\"htmlwidget-e939d26ee9e2835b356a\" class=\"sparkline html-widget\"><\/span>\n<script type=\"application/json\" data-for=\"htmlwidget-e939d26ee9e2835b356a\">{\"x\":{\"values\":[38.5,42.4,61.5,104.5,166.8,89.7,85.5,97.3,76.7,62,98.3,113.6,51.3,80.1,81.1,69.8,108,143.8,106.3,120.8,149.3,30.9,71.3,111.1,88.4,133.2,131.7,39,68.4,64.7,68.5,179.8,63,134,149,83.9,44.6,71.5,59.4,80.7,69.5,100.9,113.8,61.4,236,82.7,50.7,108.3,64.8,19.6,68.2,66.2,68.8,48.7,34.8,98.6,72.9,83.4,114.5,100.7,34.5,67,49.3,62.9,73.5,9.6,120.2,114.7,61.9,124.1,116.3,74.4,53.3,36.2,56.4,64,84.6,93.2,23.4,55.8,58.2,69.1,133.7,72.2,66.4,133,59.3,67.6,125.8,101.4,144,124.4,118.7,107.3,105.4,151.4,51.3,52,111,81,89.6,76.3,99.6,52,34.9,88.8,91.1,73.6,93.2,66.3,53.2,86.8,55,65.8,204.6,138.4,150.2,80.4,162.3,101.8,111.3,40.9,50.3,114.3,59,102.1,91,42.4,135.8,74.5,61,42.8,84.5,36.3,60.4,86.1],\"options\":{\"type\":\"line\",\"height\":20,\"width\":60},\"width\":60,\"height\":20},\"evals\":[],\"jsHooks\":[]}<\/script> <\/td>\n  <\/tr>\n  <tr>\n   <td style=\"text-align:right;\"> temp_celsius <\/td>\n   <td style=\"text-align:right;\"> -11.500 <\/td>\n   <td style=\"text-align:right;\"> -1.400 <\/td>\n   <td style=\"text-align:right;\"> 7.650 <\/td>\n   <td style=\"text-align:right;\"> 7.376 <\/td>\n   <td style=\"text-align:right;\"> 15.975 <\/td>\n   <td style=\"text-align:right;\"> 22.800 <\/td>\n   <td style=\"text-align:right;\"> <span id=\"htmlwidget-816fe3dae61ecaf91fec\" class=\"sparkline html-widget\"><\/span>\n<script type=\"application/json\" data-for=\"htmlwidget-816fe3dae61ecaf91fec\">{\"x\":{\"values\":[-3.7,-2.5,1.4,5.6,10.1,11.8,21.9,20.8,16.4,9.6,3.9,-6.8,-9.5,-1.4,-4.3,7.5,10.5,19.1,19.6,20.5,14.4,11.3,3.4,-0.6,-7.6,-6.5,0.9,8.9,14.6,16.2,19.7,19.2,16.6,9.4,5,-5.6,-5.5,-6.1,0.7,7.8,14.6,17.2,20.9,18.4,15.3,9.4,1.5,-1.8,-5.2,-5.2,2,8.8,15.4,19.9,22.8,19.4,15.9,6.8,4.3,-0.2,-5.3,-6.5,-0.4,6.6,14.5,18.1,22.7,21.2,15.4,6.7,4.6,-2.8,-2.2,-6.6,-0.7,5,13.1,18.4,21.3,19.3,15,9.5,2,-9.9,-1.4,-6.6,1.8,8.1,11.5,17.7,20,19.5,15,9.1,4.8,-1.4,-6,-2.2,1.5,8.8,16.8,19.7,20.8,20.7,14.9,10.5,2,-2.7,-4.3,-4.4,-0.7,5.3,12.9,15.9,18,17.3,15,7.5,2.9,-1.7,-3.7,-7.8,-2.2,6.4,12.5,17.4,21,20.5,13.2,8,2.8,-3.2,-11.5,-8.7,-1.3,7.2],\"options\":{\"type\":\"line\",\"height\":20,\"width\":60},\"width\":60,\"height\":20},\"evals\":[],\"jsHooks\":[]}<\/script> <\/td>\n  <\/tr>\n  <tr>\n   <td style=\"text-align:right;\"> time_in_months <\/td>\n   <td style=\"text-align:right;\"> 0.00 <\/td>\n   <td style=\"text-align:right;\"> 33.75 <\/td>\n   <td style=\"text-align:right;\"> 67.50 <\/td>\n   <td style=\"text-align:right;\"> 67.50 <\/td>\n   <td style=\"text-align:right;\"> 101.25 <\/td>\n   <td style=\"text-align:right;\"> 135.00 <\/td>\n   <td style=\"text-align:right;\"> <span id=\"htmlwidget-a36b13726a52c408a89c\" class=\"sparkline html-widget\"><\/span>\n<script type=\"application/json\" data-for=\"htmlwidget-a36b13726a52c408a89c\">{\"x\":{\"values\":[0,0.999999999999091,2.00000000000091,3,3.99999999999909,5.00000000000091,6,6.99999999999909,8.00000000000091,9,9.99999999999909,11.0000000000009,12,12.9999999999991,14.0000000000009,15,15.9999999999991,17.0000000000009,18,18.9999999999991,20.0000000000009,21,21.9999999999991,23.0000000000009,24,24.9999999999991,26.0000000000009,27,27.9999999999991,29.0000000000009,30,30.9999999999991,32.0000000000009,33,33.9999999999991,35.0000000000009,36,36.9999999999991,38.0000000000009,39,39.9999999999991,41.0000000000009,42,42.9999999999991,44.0000000000009,45,45.9999999999991,47.0000000000009,48,48.9999999999991,50.0000000000009,51,51.9999999999991,53.0000000000009,54,54.9999999999991,56.0000000000009,57,57.9999999999991,59.0000000000009,60,60.9999999999991,62.0000000000009,63,63.9999999999991,65.0000000000009,66,66.9999999999991,68.0000000000009,69,69.9999999999991,71.0000000000009,72,72.9999999999991,74.0000000000009,75,75.9999999999991,77.0000000000009,78,78.9999999999991,80.0000000000009,81,81.9999999999991,83.0000000000009,84,84.9999999999991,86.0000000000009,87,87.9999999999991,89.0000000000009,90,90.9999999999991,92.0000000000009,93,93.9999999999991,95.0000000000009,96,96.9999999999991,98.0000000000009,99,99.9999999999991,101.000000000001,102,102.999999999999,104.000000000001,105,105.999999999999,107.000000000001,108,108.999999999999,110.000000000001,111,111.999999999999,113.000000000001,114,114.999999999999,116.000000000001,117,117.999999999999,119.000000000001,120,120.999999999999,122.000000000001,123,123.999999999999,125.000000000001,126,126.999999999999,128.000000000001,129,129.999999999999,131.000000000001,132,132.999999999999,134.000000000001,135],\"options\":{\"type\":\"line\",\"height\":20,\"width\":60},\"width\":60,\"height\":20},\"evals\":[],\"jsHooks\":[]}<\/script> <\/td>\n  <\/tr>\n  <tr>\n   <td style=\"text-align:right;\"> month <\/td>\n   <td style=\"text-align:right;\"> 1.000 <\/td>\n   <td style=\"text-align:right;\"> 3.000 <\/td>\n   <td style=\"text-align:right;\"> 6.000 <\/td>\n   <td style=\"text-align:right;\"> 6.382 <\/td>\n   <td style=\"text-align:right;\"> 9.000 <\/td>\n   <td style=\"text-align:right;\"> 12.000 <\/td>\n   <td style=\"text-align:right;\"> <span id=\"htmlwidget-c9e6037cdd7e28d667dc\" class=\"sparkline html-widget\"><\/span>\n<script type=\"application/json\" data-for=\"htmlwidget-c9e6037cdd7e28d667dc\">{\"x\":{\"values\":[1,2,3,4,5,6,7,8,9,10,11,12,1,2,3,4,5,6,7,8,9,10,11,12,1,2,3,4,5,6,7,8,9,10,11,12,1,2,3,4,5,6,7,8,9,10,11,12,1,2,3,4,5,6,7,8,9,10,11,12,1,2,3,4,5,6,7,8,9,10,11,12,1,2,3,4,5,6,7,8,9,10,11,12,1,2,3,4,5,6,7,8,9,10,11,12,1,2,3,4,5,6,7,8,9,10,11,12,1,2,3,4,5,6,7,8,9,10,11,12,1,2,3,4,5,6,7,8,9,10,11,12,1,2,3,4],\"options\":{\"type\":\"line\",\"height\":20,\"width\":60},\"width\":60,\"height\":20},\"evals\":[],\"jsHooks\":[]}<\/script> <\/td>\n  <\/tr>\n  <tr>\n   <td style=\"text-align:right;\"> year <\/td>\n   <td style=\"text-align:right;\"> 1983 <\/td>\n   <td style=\"text-align:right;\"> 1985 <\/td>\n   <td style=\"text-align:right;\"> 1988 <\/td>\n   <td style=\"text-align:right;\"> 1988 <\/td>\n   <td style=\"text-align:right;\"> 1991 <\/td>\n   <td style=\"text-align:right;\"> 1994 <\/td>\n   <td style=\"text-align:right;\"> <span id=\"htmlwidget-2ac032240dd2c197b593\" class=\"sparkline html-widget\"><\/span>\n<script type=\"application/json\" data-for=\"htmlwidget-2ac032240dd2c197b593\">{\"x\":{\"values\":[1983,1983,1983,1983,1983,1983,1983,1983,1983,1983,1983,1983,1984,1984,1984,1984,1984,1984,1984,1984,1984,1984,1984,1984,1985,1985,1985,1985,1985,1985,1985,1985,1985,1985,1985,1985,1986,1986,1986,1986,1986,1986,1986,1986,1986,1986,1986,1986,1987,1987,1987,1987,1987,1987,1987,1987,1987,1987,1987,1987,1988,1988,1988,1988,1988,1988,1988,1988,1988,1988,1988,1988,1989,1989,1989,1989,1989,1989,1989,1989,1989,1989,1989,1989,1990,1990,1990,1990,1990,1990,1990,1990,1990,1990,1990,1990,1991,1991,1991,1991,1991,1991,1991,1991,1991,1991,1991,1991,1992,1992,1992,1992,1992,1992,1992,1992,1992,1992,1992,1992,1993,1993,1993,1993,1993,1993,1993,1993,1993,1993,1993,1993,1994,1994,1994,1994],\"options\":{\"type\":\"line\",\"height\":20,\"width\":60},\"width\":60,\"height\":20},\"evals\":[],\"jsHooks\":[]}<\/script> <\/td>\n  <\/tr>\n  <tr>\n   <td style=\"text-align:right;\"> temp_fahrenheit <\/td>\n   <td style=\"text-align:right;\"> 11.30 <\/td>\n   <td style=\"text-align:right;\"> 29.48 <\/td>\n   <td style=\"text-align:right;\"> 45.77 <\/td>\n   <td style=\"text-align:right;\"> 45.28 <\/td>\n   <td style=\"text-align:right;\"> 60.76 <\/td>\n   <td style=\"text-align:right;\"> 73.04 <\/td>\n   <td style=\"text-align:right;\"> <span id=\"htmlwidget-bb80b10d6c9f08ec7216\" class=\"sparkline html-widget\"><\/span>\n<script type=\"application/json\" data-for=\"htmlwidget-bb80b10d6c9f08ec7216\">{\"x\":{\"values\":[25.34,27.5,34.52,42.08,50.18,53.24,71.42,69.44,61.52,49.28,39.02,19.76,14.9,29.48,24.26,45.5,50.9,66.38,67.28,68.9,57.92,52.34,38.12,30.92,18.32,20.3,33.62,48.02,58.28,61.16,67.46,66.56,61.88,48.92,41,21.92,22.1,21.02,33.26,46.04,58.28,62.96,69.62,65.12,59.54,48.92,34.7,28.76,22.64,22.64,35.6,47.84,59.72,67.82,73.04,66.92,60.62,44.24,39.74,31.64,22.46,20.3,31.28,43.88,58.1,64.58,72.86,70.16,59.72,44.06,40.28,26.96,28.04,20.12,30.74,41,55.58,65.12,70.34,66.74,59,49.1,35.6,14.18,29.48,20.12,35.24,46.58,52.7,63.86,68,67.1,59,48.38,40.64,29.48,21.2,28.04,34.7,47.84,62.24,67.46,69.44,69.26,58.82,50.9,35.6,27.14,24.26,24.08,30.74,41.54,55.22,60.62,64.4,63.14,59,45.5,37.22,28.94,25.34,17.96,28.04,43.52,54.5,63.32,69.8,68.9,55.76,46.4,37.04,26.24,11.3,16.34,29.66,44.96],\"options\":{\"type\":\"line\",\"height\":20,\"width\":60},\"width\":60,\"height\":20},\"evals\":[],\"jsHooks\":[]}<\/script> <\/td>\n  <\/tr>\n  <tr>\n   <td style=\"text-align:right;\"> consump1M <\/td>\n   <td style=\"text-align:right;\"> 31.80 <\/td>\n   <td style=\"text-align:right;\"> 49.53 <\/td>\n   <td style=\"text-align:right;\"> 55.38 <\/td>\n   <td style=\"text-align:right;\"> 57.27 <\/td>\n   <td style=\"text-align:right;\"> 63.76 <\/td>\n   <td style=\"text-align:right;\"> 105.21 <\/td>\n   <td style=\"text-align:right;\"> <span id=\"htmlwidget-6d880c2d81783e187d2a\" class=\"sparkline html-widget\"><\/span>\n<script type=\"application/json\" data-for=\"htmlwidget-6d880c2d81783e187d2a\">{\"x\":{\"values\":[39.38025,43.352569,48.804249,38.577311,51.736944,52.592569,49.67576,72.247376,68.211427,64.519626,60.172503,44.675551,45.603722,43.944919,51.504681,37.701628,50.134461,57.587275,51.753941,63.703945,55.371629,51.014093,68.991433,31.802013,44.676726,51.618995,45.157664,44.669689,56.233606,52.09123,72.125308,71.545973,62.253865,59.311339,56.953858,40.357964,53.587755,50.620499,45.87511,50.446548,55.391987,51.516196,70.189242,58.792913,59.448721,59.808175,61.217663,41.021367,61.416921,39.484107,56.123187,43.833755,53.319837,79.312266,86.574777,80.261369,78.863607,64.434798,60.472726,47.585042,54.208204,58.403597,56.475322,47.668697,52.348045,69.32388,100.582658,105.205178,73.405903,61.76198,59.935159,45.708957,65.553063,51.251343,58.300163,47.89186,50.954604,62.076389,70.359185,97.653444,78.878241,76.009331,61.751053,42.508235,60.534297,53.488702,56.621147,52.218845,62.804935,61.454213,70.416701,76.855594,59.221683,64.519626,60.172503,43.375551,47.525506,54.77886,44.317601,53.260373,53.677209,59.023305,73.000172,75.435772,74.390116,64.866052,50.131642,49.599322,49.64002,49.314376,55.999172,44.696022,55.942755,66.369104,64.497024,55.969723,63.944781,50.290335,50.956222,48.40048,47.660472,43.927473,50.841346,45.565837,51.743702,58.903973,64.189897,64.024299,74.033281,52.833385,51.632411,47.467431,44.346357,53.317327,55.67509,46.374745],\"options\":{\"type\":\"line\",\"height\":20,\"width\":60},\"width\":60,\"height\":20},\"evals\":[],\"jsHooks\":[]}<\/script> <\/td>\n  <\/tr>\n  <tr>\n   <td style=\"text-align:right;\"> consumers1k <\/td>\n   <td style=\"text-align:right;\"> 23.89 <\/td>\n   <td style=\"text-align:right;\"> 29.13 <\/td>\n   <td style=\"text-align:right;\"> 31.25 <\/td>\n   <td style=\"text-align:right;\"> 31.39 <\/td>\n   <td style=\"text-align:right;\"> 33.10 <\/td>\n   <td style=\"text-align:right;\"> 39.65 <\/td>\n   <td style=\"text-align:right;\"> <span id=\"htmlwidget-67858d9fdbb449b6971a\" class=\"sparkline html-widget\"><\/span>\n<script type=\"application/json\" data-for=\"htmlwidget-67858d9fdbb449b6971a\">{\"x\":{\"values\":[24.963,27.38,32.588,25.511,32.313,31.858,23.929,30.452,28.688,24.936,34.078,25.601,27.238,28.126,32.845,26.001,29.377,33.275,28.72,31.159,23.885,29.233,39.031,25.918,27.885,28.695,30.102,30.11,32.692,28.224,30.919,30.229,28.395,32.274,32.992,26.956,28.996,28.154,27.478,32.72,30.466,27.618,29.949,29.821,28.098,32.021,29.145,29.428,28.863,28.687,33.043,29.092,28.731,34.249,31.617,30.542,30.42,32.422,33.084,29.167,30.164,32.655,34.654,29.57,29.096,33.523,30.801,35.274,31.433,32.584,32.625,27.569,36.909,30.822,36.301,32.327,33.149,33.027,29.178,35.109,29.988,38.742,38.934,28.986,35.78,31.676,35.069,33.238,36.737,32.407,33.399,37.719,30.027,37.644,39.654,28.707,28.942,31.08,26.923,32.836,32,30.802,31.344,32.409,33.971,33.584,30.248,30.469,30.505,31.112,31.784,31.682,30.539,36.502,32.139,30.369,36.918,32.316,33.333,30.016,30.559,31.478,35.299,30.442,32.145,35.36,34.61,33.873,37.132,32.695,34.048,32.256,27.517,33.454,38.539,31.775],\"options\":{\"type\":\"line\",\"height\":20,\"width\":60},\"width\":60,\"height\":20},\"evals\":[],\"jsHooks\":[]}<\/script> <\/td>\n  <\/tr>\n<\/tbody>\n<\/table>"},"evals":[],"jsHooks":[]}</script>
<!--/html_preserve-->
The dependent variable is ***water\_consump***, residential water consumption for the city of London between January 1983 through April 1994. ***water\_consump*** is the consumption for the residential consumers who had their meter read in the given month for the last two months, and is considered a proxy for the total residential water consumption. The independent variables are ***num\_consumers*** total residential consumers who had their water meters read in the given month for the last two months, ***precipitation\_ml*** monthly precipitation in millimeters, and ***temp\_celsius*** temperature in celsius. The number of residential consumers and residential water consumption were missing for June 1988 and an estimate (via intervention analysis) was provided in the dataset. ***Time in months*** has a correlation coefficient of 0.9961 with year, and was used in lieu of year as the trend.

![](Time-Series---Res.-Water-Consumption_files/figure-markdown_github/unnamed-chunk-6-1.png)

# Graphs of the relationships between X and Y; and expectations of X and Y relationship

![](Time-Series---Res.-Water-Consumption_files/figure-markdown_github/unnamed-chunk-7-1.png)

The XY plot of number of consumers and water consumption shows a positive trend between consumption and customers as I expected. I expect the line of best fit to have a positive coefficient.

![](Time-Series---Res.-Water-Consumption_files/figure-markdown_github/unnamed-chunk-8-1.png) The XY plot of precipitation in milliliters and water consumption shows a flat to slightly negative trend. I did not think precipitation would have any effect on residential water consumption unless a significant amount of individuals with water meter accounts had a reason to adjust their consumption such as rain water collections. I expect a coefficient close to zero and possibly negative. ![](Time-Series---Res.-Water-Consumption_files/figure-markdown_github/unnamed-chunk-9-1.png)

The XY plot of temperature in celsius and water consumption shows a positive trend between consumption and temperature. I expect that in hotter months, individuals would need and want more water; and that temperature would have a positive coefficient.

![](Time-Series---Res.-Water-Consumption_files/figure-markdown_github/unnamed-chunk-10-1.png)

The XY plot of month and water consumption shows what appears to be seasonality in consumption. There appears to be nonlinear and to arc or curve, with colder months (12, 1, 2, and 3) having lower values, while warmer months (6, 7, 8, 9 and 10) having some of the highest values of consumption. I expect the coefficient to be approximately positive and statistically insignificant.

![](Time-Series---Res.-Water-Consumption_files/figure-markdown_github/unnamed-chunk-11-1.png)

The XY plot of time in months and water consumption shows a slight trend and random scattering of points. I expect the coefficient to be approximately positive and statistically insignificant.

![](Time-Series---Res.-Water-Consumption_files/figure-markdown_github/unnamed-chunk-12-1.png)

# Model 1: A simple time series regression, with one X and no trend

    ## 
    ## Call:
    ## lm(formula = water_consump ~ num_consumers, data = tsdl_london.ts)
    ## 
    ## Residuals:
    ##       Min        1Q    Median        3Q       Max 
    ## -16002273  -7210457  -2911799   4256803  44333583 
    ## 
    ## Coefficients:
    ##                Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept)   2981010.0  9255637.4   0.322    0.748    
    ## num_consumers    1729.4      293.3   5.896 2.87e-08 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 11020000 on 134 degrees of freedom
    ## Multiple R-squared:  0.206,  Adjusted R-squared:  0.2001 
    ## F-statistic: 34.76 on 1 and 134 DF,  p-value: 2.871e-08

An OLS on water consumption (***water\_consump***) and the number of customers (***num\_consumers***) is statistically significant at the probability value (p-value) = 0.001. The coefficient for the number of customers is 1729.4. For every customer in the simple OLS time series regression, all else equal, monthly consumption is expected to increase approximately 1729.4 milliliters. The intercept coefficient is 2981010.0, the expected value of water consumption if there were no consumers, and statistically insignificant (p-value &gt; 0.1). The adjusted R-squared of model 1 is 0.2001

# Model 2: A time series regression with one X and trend

    ## 
    ## Call:
    ## lm(formula = water_consump ~ num_consumers + time_in_months, 
    ##     data = tsdl_london.ts)
    ## 
    ## Residuals:
    ##       Min        1Q    Median        3Q       Max 
    ## -16650312  -7333497  -3406872   4602234  44417578 
    ## 
    ## Coefficients:
    ##                  Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept)    -2094949.2  9637920.1  -0.217    0.828    
    ## num_consumers      1991.8      327.8   6.076 1.22e-08 ***
    ## time_in_months   -46821.8    26895.4  -1.741    0.084 .  
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 10930000 on 133 degrees of freedom
    ## Multiple R-squared:  0.2237, Adjusted R-squared:  0.212 
    ## F-statistic: 19.16 on 2 and 133 DF,  p-value: 4.878e-08

For Model 2, I added to Model 1 a trend variable ***time\_in\_months***, which counts the months from 1983, with January 1983 as 1, and April 1994 as 136. For Model 2, the intercept coefficient is -2048127.4, the expected value of water consumption if there were no consumers, all else equal, and statistically insignificant (p-value &gt; 0.1). The coefficient for the number of customers is 1991.8, statistically significant (p-value &lt; 0.001). For every additional customer, monthly consumption is expected to increase approximately 1992 milliliters. The time in months coefficient is -46821.8 which indicates that for each additional month in the series, all else equal, water consumption is expected to decrease by -46821.8 milliliters, and statistically insignificant (p-value &gt; 0.1). The adjusted R-squared of model 2 is 0.212

    ## Analysis of Variance Table
    ## 
    ## Model 1: water_consump ~ num_consumers
    ## Model 2: water_consump ~ num_consumers + time_in_months
    ##   Res.Df        RSS Df  Sum of Sq      F  Pr(>F)  
    ## 1    134 1.6266e+16                               
    ## 2    133 1.5903e+16  1 3.6239e+14 3.0307 0.08402 .
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1

A F-test of the nested model shows no statistically significant difference between models 1 and 2. The p-value for the unrestricted model “Model 2” is greater than 0.5 but less than 0.1, which indicates that probability of obtaining a F statistic of 3.0307 or larger due to random sampling is less than 1 in 10.

# Model 3: A time series regression with many Xs and trend; autocorrelation diagnostics

    ## 
    ## Call:
    ## lm(formula = water_consump ~ num_consumers + precipitation_ml + 
    ##     temp_celsius + month + time_in_months, data = tsdl_london.ts)
    ## 
    ## Residuals:
    ##       Min        1Q    Median        3Q       Max 
    ## -13553466  -6499990  -1156825   4829953  33919867 
    ## 
    ## Coefficients:
    ##                  Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept)       6261848    7690950   0.814    0.417    
    ## num_consumers        1540        259   5.945 2.40e-08 ***
    ## precipitation_ml   -29610      20690  -1.431    0.155    
    ## temp_celsius       727113      82128   8.853 5.39e-15 ***
    ## month              235720     229864   1.025    0.307    
    ## time_in_months     -24651      21043  -1.171    0.244    
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 8488000 on 130 degrees of freedom
    ## Multiple R-squared:  0.5428, Adjusted R-squared:  0.5252 
    ## F-statistic: 30.86 on 5 and 130 DF,  p-value: < 2.2e-16

## Water consumption = number of consumers + precipitation in ml + temperature in celsius + month + time in months + error

For Model 3, I started with Model 2 and added: monthly preciptation ***precipitation\_ml***, monthly temperature ***temp\_celsius***, and a variable to mark the month of the year (January as 1, December as 12) ***month***. For Model 3, the intercept coefficient is 6286500 and it is the expected value of water consumption if there were no consumers, all else equal, and statistically insignificant (p-value &gt; 0.1). The coefficient for the number of customers is 1540, statistically significant (p-value &lt; 0.001). For every additional customer, all else equal, monthly consumption is expected to increase approximately 1540. The monthly precipitation coefficient is -29610, the expected change in water consumption per one unit increase in precipitation, all else equal, and statistically insignificant (p-value &gt; 0.1). The coefficient for the temperature is 727113, the expected change in water consumption per one unit increase in temperature, all else equal and statistically significant (p-value &lt; 0.001). The month coefficient is 235720, the expected change in water consumption per one unit increase in months, all else equal, and statistically insignificant (p-value &gt; 0.1). The time in months coefficient is -24651 which indicates that for each additional month in the series, all else equal, water consumption is expected to decrease by -24651, statistically insignificant (p-value &gt; 0.1). The adjusted R-squared of model 3 is 0.5252.

![](Time-Series---Res.-Water-Consumption_files/figure-markdown_github/unnamed-chunk-17-1.png)![](Time-Series---Res.-Water-Consumption_files/figure-markdown_github/unnamed-chunk-17-2.png)![](Time-Series---Res.-Water-Consumption_files/figure-markdown_github/unnamed-chunk-17-3.png)![](Time-Series---Res.-Water-Consumption_files/figure-markdown_github/unnamed-chunk-17-4.png)

Residual plots consistently show the months July 1988, August 1988, and August 1989 to be high consumption values for the model. A rough eyeballing of the data values for August in 1988 and 1989 show a higher than average number of consumers and that August was the hottest of the time series in 1988 and about average in 1989. It is not clear why consumption was so high but is interesting to note and to consider interpolating for model refinement. Although the Town of Camelford, England had a water pollution incident in July 1988, it is not clear how the pollution incident is related to the City of London's water consumption given their approximate distance (230 miles/ 370 kilometers).

    ## 
    ##  Shapiro-Wilk normality test
    ## 
    ## data:  model_3$residuals
    ## W = 0.92446, p-value = 1.218e-06

The QQ plot suggests that residuals are not normally distributed. The Shapiro-Wilk normality test (p-value &lt; 0.001) reveals the residuals are not normally distributed, which indicate that this an OLS model might be inadequate.

    ## 
    ##  studentized Breusch-Pagan test
    ## 
    ## data:  model_3
    ## BP = 10.853, df = 5, p-value = 0.05437

### BP test

BP statistic of 10.853 with a p-value of 0.05437. This indicates heteroskedasticity in the errors at the p &lt; 0.1 level but not at p &lt; 0.05 level. This suggests that we should use heteroskedastic robust standard errors.

    ##    num_consumers precipitation_ml     temp_celsius            month 
    ##         1.313893         1.130409         1.157493         1.202792 
    ##   time_in_months 
    ##         1.288236

### VIF interpretation

The VIF for all variables in Model 3 are all positive and between, 1 and less than 1.5. The VIF indicates whether multicollinearity exists due to a particular independent variable. The VIF test indicates no multicollinearity among the independent variables.

### ACF plot of the residuals for Model 3

![](Time-Series---Res.-Water-Consumption_files/figure-markdown_github/unnamed-chunk-22-1.png)![](Time-Series---Res.-Water-Consumption_files/figure-markdown_github/unnamed-chunk-22-2.png)

The ACF plot/correlogram shows autocorrelation (AR) at lag 1 and general autoregressive tendencies and seasonality in the lags; with the AR flipping approximately every 3 months of lag, for the two years of lag shown. AR1 indicates possible unit roots in the data generating process, unit roots indicate the data generating process mimics a random walk and would need to be corrected. A random walk is a non-stationary process with no specified mean or variance. However, the increments in a random walk process follow a white noise process, which is stable and stationary with a mean of zero.

This indicates that the OLS standard errors of the model are predictable over time, a major violation of the Gauss-Markov assumptions that errors are i.i.d. independent identically distributed. The ACF plot also shows non-stationarity of the errors, that is the mean and variance of the errors fluctuate over time (and are not stationary or constant).

### Durbin-Watson and Breusch-Godfrey tests

    ## 
    ##  Durbin-Watson test
    ## 
    ## data:  model_3
    ## DW = 1.072, p-value = 8.306e-09
    ## alternative hypothesis: true autocorrelation is greater than 0

    ## 
    ##  Breusch-Godfrey test for serial correlation of order up to 1
    ## 
    ## data:  model_3
    ## LM test = 31.243, df = 1, p-value = 2.276e-08

Unsurprisingly the Durbin-Watson and Breusch-Godfrey tests show strong statistically significance for autocorrelation. We should use autocorrelation robust standard errors, such as Newey-West standard errors, to address the serial correlation of the lags and heteroskedasticity of the Model 3.

The Newey-West standard errors for model 3:

    ## 
    ## t test of coefficients:
    ## 
    ##                    Estimate Std. Error t value  Pr(>|t|)    
    ## (Intercept)      6261848.45 5387480.86  1.1623    0.2472    
    ## num_consumers       1539.94     168.51  9.1384 1.089e-15 ***
    ## precipitation_ml  -29610.49   23790.67 -1.2446    0.2155    
    ## temp_celsius      727112.70  152710.11  4.7614 5.050e-06 ***
    ## month             235719.97  227339.01  1.0369    0.3017    
    ## time_in_months    -24651.40   26401.85 -0.9337    0.3522    
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1

For comparison, the OLS standard errors for model 3 are:

    ## 
    ## Call:
    ## lm(formula = water_consump ~ num_consumers + precipitation_ml + 
    ##     temp_celsius + month + time_in_months, data = tsdl_london.ts)
    ## 
    ## Residuals:
    ##       Min        1Q    Median        3Q       Max 
    ## -13553466  -6499990  -1156825   4829953  33919867 
    ## 
    ## Coefficients:
    ##                  Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept)       6261848    7690950   0.814    0.417    
    ## num_consumers        1540        259   5.945 2.40e-08 ***
    ## precipitation_ml   -29610      20690  -1.431    0.155    
    ## temp_celsius       727113      82128   8.853 5.39e-15 ***
    ## month              235720     229864   1.025    0.307    
    ## time_in_months     -24651      21043  -1.171    0.244    
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 8488000 on 130 degrees of freedom
    ## Multiple R-squared:  0.5428, Adjusted R-squared:  0.5252 
    ## F-statistic: 30.86 on 5 and 130 DF,  p-value: < 2.2e-16

# First differenced time series regression

    ## 
    ## Call:
    ## lm(formula = firstD(water_consump) ~ firstD(num_consumers) + 
    ##     firstD(precipitation_ml) + firstD(temp_celsius) + firstD(month) + 
    ##     time_in_months, data = tsdl_london.ts)
    ## 
    ## Residuals:
    ##       Min        1Q    Median        3Q       Max 
    ## -20325424  -4665378    131133   3907859  31123799 
    ## 
    ## Coefficients:
    ##                           Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept)               314630.4  1373921.8   0.229  0.81923    
    ## firstD(num_consumers)       1567.0      161.6   9.697  < 2e-16 ***
    ## firstD(precipitation_ml)   21320.4    13997.4   1.523  0.13016    
    ## firstD(temp_celsius)      611571.5   126475.3   4.836 3.71e-06 ***
    ## firstD(month)            -713882.3   214066.1  -3.335  0.00111 ** 
    ## time_in_months             -5631.2    17527.6  -0.321  0.74852    
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 7929000 on 129 degrees of freedom
    ##   (1 observation deleted due to missingness)
    ## Multiple R-squared:  0.5417, Adjusted R-squared:  0.524 
    ## F-statistic:  30.5 on 5 and 129 DF,  p-value: < 2.2e-16

### First differenced model interpretation

As we previously saw, Model 3 is non-stationary. Differencing should address this. For the first differenced version of Model 3, the intercept coefficient is 314630.4, the expected value of water consumption if there were no consumers, all else equal, and statistically insignificant (p-value &gt; 0.1). The coefficient for the number of customers is 1567, statistically significant (p-value &lt; 0.001). For every additional customer, all else equal, monthly consumption is expected to increase approximately 1567 milliliters. The monthly precipitation coefficient is 21320.4, the expected change in water consumption per one unit increase in precipitation, all else equal, and statistically insignificant (p-value &gt; 0.1). The coefficient for the temperature is 611571.5, the expected change in water consumption per one unit increase in temperature, all else equal and statistically significant (p-value &lt; 0.001). The month coefficient is -713882.3, the expected change in water consumption per one unit increase in months, all else equal, and statistically significant (p-value &lt; 0.01). The time in months coefficient is -5631.2 which indicates that for each additional month in the series, all else equal, water consumption is expected to decrease by -5631.2 milliliters and statistically insignificant (p-value = 0.1). The adjusted R-squared of the first differenced model 3 is 0.524. ![](Time-Series---Res.-Water-Consumption_files/figure-markdown_github/unnamed-chunk-29-1.png)![](Time-Series---Res.-Water-Consumption_files/figure-markdown_github/unnamed-chunk-29-2.png)

The ACF plot of the first differenced model 3 still shows fluctuating positive and negative autocorrelations, including statistical significance in the 3rd, 9th, 12th, 11th and 15th lags, however it shows fewer lags with statistically significant autocorrelations than the OLS model 3. The first differenced model indicates a reduction in the autocorrelation and a better fit.

### Durbin-Watson and Breusch-Godfrey tests - First differenced Model 3

    ## 
    ##  Durbin-Watson test
    ## 
    ## data:  lm.Dmodel_3
    ## DW = 2.1156, p-value = 0.7361
    ## alternative hypothesis: true autocorrelation is greater than 0

    ## 
    ##  Breusch-Godfrey test for serial correlation of order up to 1
    ## 
    ## data:  lm.Dmodel_3
    ## LM test = 0.46349, df = 1, p-value = 0.496

The Durbin-Watson and Breusch-Godfrey tests of the first differences also showed improvement in autocorrelation and removed the statistical significance of autocorrelation in the errors.

    ## 
    ##  Breusch-Godfrey test for serial correlation of order up to 1
    ## 
    ## data:  lm.Dmodel_3
    ## LM test = 0.46349, df = 1, p-value = 0.496

    ## 
    ##  Breusch-Godfrey test for serial correlation of order up to 2
    ## 
    ## data:  lm.Dmodel_3
    ## LM test = 0.59349, df = 2, p-value = 0.7432

    ## 
    ##  Breusch-Godfrey test for serial correlation of order up to 3
    ## 
    ## data:  lm.Dmodel_3
    ## LM test = 19.922, df = 3, p-value = 0.0001762

    ## 
    ##  Breusch-Godfrey test for serial correlation of order up to 4
    ## 
    ## data:  lm.Dmodel_3
    ## LM test = 28.664, df = 4, p-value = 9.147e-06

    ## 
    ##  Breusch-Godfrey test for serial correlation of order up to 5
    ## 
    ## data:  lm.Dmodel_3
    ## LM test = 31.543, df = 5, p-value = 7.318e-06

    ## 
    ##  Breusch-Godfrey test for serial correlation of order up to 6
    ## 
    ## data:  lm.Dmodel_3
    ## LM test = 31.595, df = 6, p-value = 1.951e-05

    ## 
    ##  Breusch-Godfrey test for serial correlation of order up to 7
    ## 
    ## data:  lm.Dmodel_3
    ## LM test = 45.36, df = 7, p-value = 1.164e-07

    ## 
    ##  Breusch-Godfrey test for serial correlation of order up to 8
    ## 
    ## data:  lm.Dmodel_3
    ## LM test = 47.472, df = 8, p-value = 1.246e-07

    ## 
    ##  Breusch-Godfrey test for serial correlation of order up to 9
    ## 
    ## data:  lm.Dmodel_3
    ## LM test = 55.98, df = 9, p-value = 7.918e-09

    ## 
    ##  Breusch-Godfrey test for serial correlation of order up to 10
    ## 
    ## data:  lm.Dmodel_3
    ## LM test = 61.305, df = 10, p-value = 2.051e-09

    ## 
    ##  Breusch-Godfrey test for serial correlation of order up to 11
    ## 
    ## data:  lm.Dmodel_3
    ## LM test = 63.4, df = 11, p-value = 2.152e-09

    ## 
    ##  Breusch-Godfrey test for serial correlation of order up to 12
    ## 
    ## data:  lm.Dmodel_3
    ## LM test = 63.478, df = 12, p-value = 5.204e-09

Breusch-Godfrey tests do however show that autocorrelation remains are higher orders, specifically for all lags that between 3 and 12 lags of the model.

# Check for unit roots.

Augmented Dicky Fuller (DF) tests were conducted to test for unit roots in the underlying variables. The DF test of the dependent variable (water consumption) indicates that we cannot reject the null of unit roots at 6 lags of the variable (i.e. there are unit roots that need to be addressed).

    ## 
    ## Title:
    ##  Augmented Dickey-Fuller Test
    ## 
    ## Test Results:
    ##   PARAMETER:
    ##     Lag Order: 1
    ##   STATISTIC:
    ##     Dickey-Fuller: -0.8472
    ##   P VALUE:
    ##     0.3439 
    ## 
    ## Description:
    ##  Thu Jan 21 22:38:44 2021 by user:

    ## 
    ## Title:
    ##  Augmented Dickey-Fuller Test
    ## 
    ## Test Results:
    ##   PARAMETER:
    ##     Lag Order: 2
    ##   STATISTIC:
    ##     Dickey-Fuller: -0.8802
    ##   P VALUE:
    ##     0.3333 
    ## 
    ## Description:
    ##  Thu Jan 21 22:38:44 2021 by user:

    ## 
    ## Title:
    ##  Augmented Dickey-Fuller Test
    ## 
    ## Test Results:
    ##   PARAMETER:
    ##     Lag Order: 3
    ##   STATISTIC:
    ##     Dickey-Fuller: -0.8286
    ##   P VALUE:
    ##     0.3498 
    ## 
    ## Description:
    ##  Thu Jan 21 22:38:44 2021 by user:

    ## 
    ## Title:
    ##  Augmented Dickey-Fuller Test
    ## 
    ## Test Results:
    ##   PARAMETER:
    ##     Lag Order: 4
    ##   STATISTIC:
    ##     Dickey-Fuller: -0.6352
    ##   P VALUE:
    ##     0.4114 
    ## 
    ## Description:
    ##  Thu Jan 21 22:38:44 2021 by user:

    ## 
    ## Title:
    ##  Augmented Dickey-Fuller Test
    ## 
    ## Test Results:
    ##   PARAMETER:
    ##     Lag Order: 5
    ##   STATISTIC:
    ##     Dickey-Fuller: -0.5224
    ##   P VALUE:
    ##     0.4474 
    ## 
    ## Description:
    ##  Thu Jan 21 22:38:44 2021 by user:

    ## 
    ## Title:
    ##  Augmented Dickey-Fuller Test
    ## 
    ## Test Results:
    ##   PARAMETER:
    ##     Lag Order: 6
    ##   STATISTIC:
    ##     Dickey-Fuller: -0.5637
    ##   P VALUE:
    ##     0.4342 
    ## 
    ## Description:
    ##  Thu Jan 21 22:38:44 2021 by user:

Interestingly, the Ljung-Box "white-noise" tests, for the residuals in models 1, 2, and 3 do not indicate unit roots.

    ## 
    ##  Box-Ljung test
    ## 
    ## data:  resid(model_3)
    ## X-squared = 285.29, df = 40, p-value < 2.2e-16

    ## 
    ##  Box-Ljung test
    ## 
    ## data:  resid(model_2)
    ## X-squared = 679.45, df = 40, p-value < 2.2e-16

    ## 
    ##  Box-Ljung test
    ## 
    ## data:  resid(model_1)
    ## X-squared = 682.5, df = 40, p-value < 2.2e-16

# Auto ARIMA on the residuals of Model 2

    ## Series: model_2$residuals 
    ## ARIMA(3,0,1) with zero mean 
    ## 
    ## Coefficients:
    ##          ar1     ar2      ar3    ma1
    ##       0.2434  0.4504  -0.4579  0.613
    ## s.e.  0.2233  0.1512   0.0760  0.271
    ## 
    ## sigma^2 estimated as 5.165e+13:  log likelihood=-2338.74
    ## AIC=4687.47   AICc=4687.93   BIC=4702.04

The auto.ARIMA function indcates that an ARIMA of 3,0,1 is the best structure for the errors in Model 2. This indicates the errors in Model 2 should have an autocorrelation (AR) correction of 3 (i.e that a lag of 3 in the errors for the dependent variable (water consumption)), no differencing, and a moving average correction of 1 (i.e. 1 lag of the errors from the first lag (moving average of 1)). The AIC (4687.47) is the measure of model fit and the higher the AIC the better the model.

The arima() function generated an error **"Error in solve.default(res$hessian \* n.used, A) : system is computationally singular: reciprocal condition number = 1.66623e-16"** when I went to see the coefficients (weights for each lag) auto.ARIMA() suggested for Model 2. I reduced the scale of the y-variable ***water\_consump*** data by 10, which provided accurate weights for the y-variable lags (ar1, ar2, and ar3) and for the moving average (ma1).

    ## 
    ## Call:
    ## arima(x = y_scaled_by_10, order = c(3, 0, 1), xreg = xvars_m2)
    ## 
    ## Coefficients:
    ##          ar1     ar2      ar3     ma1  intercept  num_consumers  time_in_months
    ##       0.2298  0.4696  -0.4565  0.6382   235558.3       183.8041       -4093.530
    ## s.e.  0.2370  0.1573   0.0769  0.2915   454542.1        12.9877        3359.364
    ## 
    ## sigma^2 estimated as 4.961e+11:  log likelihood = -2024.89,  aic = 4065.78
    ## 
    ## Training set error measures:
    ##                    ME     RMSE      MAE       MPE     MAPE      MASE
    ## Training set 204.7861 704332.8 498967.3 -1.319632 8.803194 0.5615818
    ##                     ACF1
    ## Training set -0.03028363

The coefficients for ***ar1*** (0.2298, s.e. = 0.237), ***ar2*** (0.4696, s.e. = 0.1573), and ***ar3*** (-0.4565, s.e. = 0.0769) are the weights for the first, second and third lags of the dependent variable, ***water\_consump***. The coefficient for ***ma1*** (0.6382, s.e. = 0.2914) is the weighted sum of the current and lagged errors, for a lag of 1. After reducing the scale o the y-variable ***water\_consump*** data by 10, the coefficients for the intercept, number of consumers and time in months variables were also reduced by a scale of 10. After returning to the normal scale, the coefficient for the ***intercept*** is 2396518 (s.e. = 4554258), and it represents the water consumption, when all other independent variables are zero. After returning to the normal scale, the coefficient for ***num\_consumers*** is 1838.041 (s.e. = 129.852), and it represents the change in water consumption, all else equal, for one unit increase in number of consumers. After returning to the normal scale, the coefficient for ***time\_in\_months*** is -40935.30 (s.e. = 33578.81), and it represents the change in water consumption, all else equal, for one unit increase in the monthly time trend.

# Auto ARIMA on the residuals of Model 3

    ## Series: model_3$residuals 
    ## ARIMA(2,0,2) with zero mean 
    ## 
    ## Coefficients:
    ##          ar1     ar2     ma1     ma2
    ##       0.3490  -0.522  0.1694  0.7238
    ## s.e.  0.1327   0.118  0.1076  0.1007
    ## 
    ## sigma^2 estimated as 4.673e+13:  log likelihood=-2331.8
    ## AIC=4673.6   AICc=4674.07   BIC=4688.17

The auto.ARIMA function indcates that an ARIMA of 2,0,2 is the best structure for the errors in Model 3. This indicates that the errors in Model 3 should have an AR correction of 2 (i.e. that a lag of 2 for the dependent variable (water consumption)), no differencing, and a moving average correction of 2 (i.e. 2 lags of the errors from the previous lags (moving average of 2)). The AIC for model 3 (4673.6) is slightly less that the AIC for model 2, indicating model 2 is the better model.

    ## 
    ## Call:
    ## arima(x = y_scaled_by_10, order = c(2, 0, 2), xreg = xvars_m3)
    ## 
    ## Coefficients:
    ##          ar1      ar2     ma1     ma2  intercept  num_consumers  time_in_months
    ##       0.4646  -0.4072  0.1954  0.6385   316368.2       166.1961       -2921.290
    ## s.e.  0.1424   0.1353  0.1138  0.1179   491785.3        14.4189        2752.791
    ##       precipitation_ml  temp_celsius      month
    ##               1159.913      63777.25  -28642.17
    ## s.e.          1203.884      11117.77   21050.03
    ## 
    ## sigma^2 estimated as 4.103e+11:  log likelihood = -2011.87,  aic = 4045.74
    ## 
    ## Training set error measures:
    ##                   ME   RMSE      MAE       MPE     MAPE      MASE       ACF1
    ## Training set 260.318 640542 484995.9 -1.175073 8.695643 0.5458571 0.01040057

The coefficients for ***ar1*** (0.4646, s.e. = 0.1424) and ***ar2*** (-0.4072, s.e. = 0.1353) are the weights for the first and second lags of the dependent variable, water consumption. The coefficient for ***ma1*** (0.1954, s.e. = 0.1138) and ***ma2*** (0.6385, s.e. = 0.1179) are the weighted sum of the current and lagged errors, for the first and second lags. After reducing the scale of the y-variable ***water\_consump*** data by 10, the coefficients for the intercept, number of consumers and time in months variables were also reduced by a scale of 10. After returning to the normal scale, the coefficient for the ***intercept*** is 3192895 (s.e. = 4921449), and it represents the water\_consumption, when all other independent variables are zero. After returning to the normal scale, the coefficient for ***num\_consumers*** is 1661.961 (s.e. = 144.190), and it represents the change in water\_consumption, all else equal, for one unit increase in number of consumers. After returning to the normal scale, the coefficient for ***time\_in\_months*** is -29212.90 (s.e. = 27527.78), and it represents the change in water\_consumption, all else equal, for one unit increase in the monthly time trend. After returning to the normal scale, the coefficient for ***precipitation\_ml*** is 11599.13 (s.e. = 12038.03), and it represents the change in water\_consumption, all else equal, for one unit increase in the precipitation by 1 unit change in precipitation\_ml (1 milliliter). After returning to the normal scale, the coefficient for ***temp\_celsius*** is 637772.5 (s.e. = 111173), and it represents the change in water\_consumption, all else equal, for one unit increase in number of consumers. After returning to the normal scale, the coefficient for ***month*** is -286421.7 (s.e. = 210510.6), and it represents the change in water\_consumption, all else equal, for one unit increase in the 12 levels for the months of the year (January = 1 ... December = 12).

    ## Series: lm.Dmodel_3$residuals 
    ## ARIMA(0,0,0) with zero mean 
    ## 
    ## sigma^2 estimated as 6.008e+13:  log likelihood=-2333.1
    ## AIC=4668.21   AICc=4668.24   BIC=4671.11

The auto.ARIMA function for the first differenced version of model 3 indicates an ARIMA (0,0,0) is the best structure for the errors in the model. This indicates no further correction to the errors for the model. The AIC is the lowest for this model (compared to for Model 2 and Model 3), however at 4668.21. For interpretation of ARIMA(0,0,0) for the First Difference Model, see the section entitled ***"First differenced model interpretation"***.
