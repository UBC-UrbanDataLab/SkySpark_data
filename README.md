# SkySpark_data
The Urban Data Lab provides access to 15-minute data on UBC buildings mirrored from the UBC EWS SkySpark platform.

## About the Data
The UBC EWS [SkySpark platform](http://energy.ubc.ca/energy-and-water-data/skyspark/), managed by Energy and Water Services (EWS) of the University of British Columbi (UBC), collects data on energy and water use, HVAC equipment, and  weather information every 15 minutes. UBC Urban Data Lab (UDL) mirrored the SkySpark database into an InfluxDB instance to increase the accessibility and usability of the data.

The InfluxDB can be accessed using [command line interface](https://docs.influxdata.com/influxdb/v1.7/tools/shell/) or [client libraries](https://docs.influxdata.com/influxdb/v1.7/tools/api_client_libraries/). The Python notebook `SKYSPARK v7 Tutorial.ipynb` is a tutorial for aceessing and exploring the data in Python 3.

A Grafana visualization is avalable [here](https://udl.grafana.net/d/bMRdlVaWz/skyspark?orgId=1&from=1576273851405&to=1576878651405&panelId=2&fullscreen). Users can click on “Panel Title” and choose “Edit”, which allows them to edit the SQL query and select a time range for the data. However, the data values are stored as strings and Grafana can only visualize the aggregated values such as `count`.

## Database Structure
![](https://github.com/UBC-UrbanDataLab/SkySpark_data/blob/master/SKYSPARK%20v7%20Structure.JPG)

**[InfluxDB concepts](https://docs.influxdata.com/influxdb/v1.8/concepts/)**: database, point, measurement, tag, field, timestamp

**Notes**
- For each data point on SkySpark, the `id` in `POINT` is the same as the `uniqueID` in `READINGS`.
- A SkySpark data point can have up to 155 tags (recorded in the `POINT` measuremt). It usually has the following tagging information so we also record these tags in `READINGS` to simplify queries.
  - siteRef:
  - groupRef:
  - typeRef:
  - equipRed:
  - navName:
  - unit:

