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

1. For each data point on SkySpark, the `id` in `POINT` is the same as the `uniqueID` in `READINGS`.
2. A SkySpark data point can have up to 155 tags (recorded in the `POINT` measuremt). Each data point usually has the following tagging information so we also record these tags in `READINGS` to simplify queries.
   * siteRef: a site is a facility (usually a building) 
   * groupRef: a group refers to a system or a floor in a building
   * typeRef: type of the data point  
   * equipRed: information on the equipment
   * navName: information interpreted and added by SkySpark managers
   * unit: the unit of the reading value
3. Each data point has the reading values stored in one of the following fields in `READINGS`:
   * val_num: float values
   * val_bool: boolean values
   * val_str: string values
   * val_unk: value format unknown and stored as string
4. Timestamps are in UTC on InfluxDB: `time` in `READINGS` is the time the reading values are recorded, and `time` in `POINT` is a constant value to allow overwriting the existing data (this happens at 2am daily).
