# Philadelphia Infill Analysis

Will Parker — Command-Line GIS with Professor Will Payne — Fall 2024 — Bloustein School @ Rutgers University


This project examines patterns of in-fill development since 2018 in Philadelphia on the city block level. It combines parcel vacancy rates in the baseline year of 2018 with residential permitting data since that year. The project combines these factors to define 4 typologies:

• Stable low-vacancy, blocks with low starting vacancy and little development

• Unique blocks, blocks with low starting vacancy yet many permits for new development

• Stable high-vacancy, blocks with high starting vacancy and little development

**•In-fill hotspots, blocks with high starting vacancy and many permits for new development**

This project emerged from the observation that many parts of Philadelphia with a high proportion of vacant lots have been filling in over the last few years, while other high-vacancy areas have remained stable. The analysis is a first step to understanding why some areas are filling in while others remain unchanged.

Data for this analysis was downloaded from Open Data Philly, courtesy of the City of Philadelphia. Vacant lot data for 2018 is current as of 2018, and the building permit data contains entries dated well into 2024. Methodology, as well as data processing and data quality issues are described at the end.

The first map produced is a bivariate choropleth showing both 2018 vacancy and permits since 2018 for the entire city. This form is particularly useful because it makes typologies very visually apparent. Most of the city falls in the grey, stable low-vacancy category. Many parts of North Philadelphia and the northern portion of West Philadelphia fall in the blue category of stable high-vacancy. A few areas with unique redevelopment circumstances have few vacant parcels and many permits, shown in lighter red -- most of these areas are being developed for residential use for the first time. Finally, incidences of the deep red in-fill category, where many vacant lots existed in 2018 and many have since been developed, are visible but occur on a small enough scale to be almost invisible on this map.

<img src="citywide_bivariate_choropleth.png" alt="Bivariate Choropleth of Philadelphia Vacancy and Permits" width="600"/>

To see this map as its own web page, click [here](citywide_bivariate_choropleth.png)

Most of these in-fill blocks fall in North Philadelphia, so a detailed map of that area is shown below. Many in-fill blocks occur just south of Cecil B Moore Avenue near 27th Street, and east of Broad Street between Cecil B Moore Avenue and Germantown Avenue. 

<img src="north_bivariate_choropleth.png" alt="Bivariate Choropleth of Philadelphia Vacancy and Permits" width="600"/>

To see this map as its own web page, click [here](north_bivariate_choropleth.png)

Explore the data yourself in the map below, where you can see the in-fill blocks (defined here as >20 vacancies in 2018, >20 permits since 2018), individual permits and vacant parcels, as well as tract-level percentage of households living in poverty (below 200% of the Federal Poverty Line). Future research could do more to discover demographic trends, examine home-ownership, and ultimately identify how in-fill development changes a neighborhood. One initial finding apparent here is that in-fill is occuring in areas of some poverty, but generally not in the city's very poorest neighborhoods.

<iframe src="phila-infill.html" height="800" width="800"></iframe>
To see this map as its own web page, click [here](phila-infill.html)



## Methodology, Data Quality, and Processing Notes

Georeferenced Building Permit data for the city of Philadelphia was available as point data for the periods 2007-2015 and 2016-present, and could be sorted for only permits authorizing new construction. Land Use data for the period 2012-2018 was also available on a parcel level as polygon data. While filtering Land Use to only vacant lands was a simple line of code, the building permit data was a substantial challenge. A project description was available for each permit, and I used this to piece together the combinations of two different fields ("permitdesc" and "typeofwork") which yielded only permits authorizing new residential construction. The meanings were not immediate obvious--for example, homes were classified as typeofwork ENTIRE, but I also had to include pre-fabricated homes using typeofwork MOD/IN.

The parcel data did not appear to have any identification of tax blocks, so I opted to instead use census blocks as a geography to aggregate both a count of vacancies and a count of permits. I pulled this geography and the city boundary directly using the pygris package. Because census blocks extend to the middle of streets while lot lines do not, all permit points and vacancy polygons fell within their blocks and very simple spatial join allowed me to count the number of each in a given block.

One issue which became apparent is that approximately 200 building permits contained a null geometry attribute. They did, however, contain a string address field. To use these entries, I isolated them from the rest of the data and exported to another notebook, where I geocoded them with ESRI Geocoder and merged them back together with the other building permits.


