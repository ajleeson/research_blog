## Salish Sea Model Nutrient Data

This week I focused on gaining a high-level understanding of Ecology's nutrient data. A subset of these data are also used as nutrient inputs to the Salish Sea Model.

These data contain nutrient loading timeseries for marine point and nonpoint sources and contain GIS data with the UTM Northing/Easting coordinates of these sources. I converted the coordinates to lat/lon and generated a spreadsheet with these information.

I have also generated a few figures to gain a high-level understanding of spatial and temporal nutrient loading trends.

Links:
- [Ecology's Salish Sea Model downloadable files](https://fortress.wa.gov/ecy/ezshare/EAP/SalishSea/SalishSeaModelBoundingScenarios.html) including nutrient timeseries and GIS data
- [SSM_source_info.xlsx](https://github.com/ajleeson/LO_user/files/9465489/SSM_source_info.xlsx), excel file with lat/lon coordinates of rivers and marine point sources

---

## Nutrient Timeseries

The timeseries data contain nutrient information from January 1999 - July 2017 in .xlsx files. They include marine point sources and nonpoint sources (such as river mouths). There are 99 total point sources and 161 total nonpoint sources.

The timeseries data are in an individual excel file for each point source and nonpoint source. Point sources have data on a monthly basis. Nonpoint sources have data on a daily basis.

Both point and nonpoint source files provide data for the same variables. However, the variable names differ slightly between the point source and nonpoint source files. A summary of the variables are provided in the table below:

|Description| Point Source Name| Nonpoint Source Name <br>(if different from point source)|
| --- | --- | --- |
|  | Date           | |
|  |  Flow, cms     | FlowRate (cms)|
|Temperature                    | Temp (C)       | |
|Salinity                       |  Saln (ppt)    | |
|Ammonium                       |  NH4 (mg/L)    | |
|Nitrate + nitrite              | NO3+NO2 (mg/L) | |
|Phosphate                      |  PO4 (mg/L)    | |
|Dissolved oxygen               | DO (mg/L)      | |
|  |  pH (SU)       | |
|Dissolved organic nitrogen     |  DON(mg/L)     | |
|Particulate organic nitrogen   |  PON(mg/L)     | |
|Dissolved organic phosphorus   |  DOP(mg/L)     | |
|Particulate organic phosphorus | POP(mg/L)      | |
|  |  POCS(mg/L)    | |
|  |  POCF(mg/L)    | |
|Refractory Particulate Organic Carbon (?)  |  POCR(mg/L)    | |
|  |  sDOC(mg/L)    |DOCS(mg/L) |
|  | fDOC (mg/L)    |DOCF(mg/L) |
|Algae species 1 Diatoms        |  Diatoms       | |
|Algae species 2 Dinoflagellates|  Dinoflag      | |
|  |  Chlorophyll   | |
|Dissolved inorganic carbon     | DIC(mmoles/m3) | |
|Alkalinity                     | Alk(mmoles/m3) | |

<br>

Note: I am still missing the definition of some of these variables and will continue to fill out this table as I find more information.

---

## Point and Nonpoint Source Coordinates

The GIS data contains many different layers, the most important being the UTM coordinates of all point and nonpoint sources. Note that the coordinates of rivers corresponds to river mouths.

I converted the UTM cooridnates to lat/lon using the pyproj packet in Python. Then I saved these results in the excel file listed at the intro of this page.

Figure 1 shows the location of all point and nonpoint sources included in this dataset.

<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/188668685-db4ea845-fa1e-4e2e-814e-47a4d842e793.png" width="600"/><br>Fig 1. Location of point and nonpoint sources in Salish Sea Model.</p><br>

---

## High Level Summary

I spent some time with the data this week to build some familiarity with the high-level trends.

First, I looked at spatial trends. For both rivers and point sources, I took the average loading from every source across the full available timeseries. I ignored zeros because point sources that went offline or came online midway through 1999-2017 were padded with zeros.

Figure 2 shows the resulting average DIN load from marine point sources. The largest dischargers appear to be in the Vancouver-area and near the Seattle-area.

<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/188669987-a5d99a57-0748-4768-bf22-7ee8b832e76d.png" width="400"/><br>Fig 2. Average DIN load of each point source.</p><br>

Figure 3 shows the resulting average DIN load from nonpoint sources. The largest discharger is in the Vancouver-area (Fraser River). There are also several relatively large rivers scattered along the east-side of the Sound.

<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/188670848-07c3162c-9676-40da-bbea-38403516636b.png" width="400"/><br>Fig 3. Average DIN load of each nonpoint source.</p><br>

I then looked at temporal patterns of point and nonpoint sources.

For every discharger I calculated the average DIN load of every month across the full timeseries. For example, the January average of a point source is the average of all January loadings between 1999-2017. Then I summed all of the dischargers to get a net average discharge to the Salish Sea, by month.

Figure 4 shows these month average net DIN loads for point sources, offset by the mean of the sum. Point sources discharge the greatest loads during Fall and Winter, and discharges the least during mid- to late-Summer.

<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/188672891-6f473ed8-2d57-4caf-8955-29939569f1f0.png" width="600"/><br>Fig 4. Month average net DIN load for point sources.</p><br>

Figure 5 shows the same type of plot for nonpoint sources. In this case, there appear to be two peaks with high DIN loading. First, there is a peak during Fall and Winter, which is similar to non-point sources. There is also a peak during late-Spring. The nutrient loading is smallest during late-Summer. 

<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/188673927-6594492c-7a44-46fd-9245-ad0b9bfc26af.png" width="600"/><br>Fig 5. Month average net DIN load for nonpoint sources.</p><br>

Figure 6 shows the month average DIN loads for each individual nonpoint source. Here, I observe that the spring peak in DIN loading is almost entirely due to just one river. This is the Fraser River.

<p style="text-align:center;"><img src="https://user-images.githubusercontent.com/15829099/188675453-45c831a9-9670-420b-9f74-3c41c0f97e52.png" width="600"/><br>Fig 5. Month average DIN load for individual nonpoint sources.</p><br>
