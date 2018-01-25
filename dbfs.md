# Dockless Bikeshare Feed Specification (DBFS)
DBFS is a proposed standard of data sharing between operators of dockless bikeshare systems and interested consumers of their data.

## Table of Contents

* [Revision History](#revision-history)
* [Introduction](#introduction)
* [Authentication and Authorization](#authentication-and-authorization)
* [API Response Format](#api-response-format)
* [Endpoint URL Format & Versioning](#endpoint-url-format-versioning)
* [Endpoints](#endpoints)
    * [GET /bikes](#get-bikes)
    * [GET /regions](#get-regions)

## Revision History
* 02/2018 - Initial Draft for DBFS V1.0

## Introduction
The specification proposed in this document is inspired by the General Bikeshare Feed Specification (GBFS), which has been widely adopted by traditional, station-based bikeshare system operators. Because dockless bikeshare is a fundamentally different product from station-based bikeshare, a new standard of data sharing is necessary. Some of the major differences between the two business models are:
* Dockless bikeshare does not employ the use of stations, so the data models must be reimagined
* Dockless bikes are generally more densely populated than station-based ones, so more attention must be paid towards managing the size of data feeds

However, DBFS is still very similar to GBFS in that only real-time or semi-real-time data is exposed, and not historical data.

## Authentication and Authorization
Each system operator will provide each client with an API key secret. On each request to one of our endpoints, clients will need to supply their given secret as a bearer token in the HTTP Authorization request header.

System operators and clients will also agree geofences representing the operational region(s) that the client is authorized to oversee. Only bikes physically located within the agreed upon geofences will be available via this API.

## API Response Format
Endpoints should use the JSON API version 1.0 format for responses (see http://jsonapi.org/format/1.0/).

## Endpoint URL Format & Versioning
Endpoint URLs should conform to the following format: {base URL}/{version string}/{relative endpoint path}
Example: https://xyzbikeshare.com/api/v1.0/bikes, where https://xyzbikeshare.com/api is the base URl, v1.0 is the version string, and /bikes is the relative endpoint path.

Versioning should be made explicit in the URL. This document describes **DBFS V1.0**.

## Endpoints
This specification defines the following endpoints along with their associated content:

Endpoint       | Required     | Defines
-------------- | ------------ | ----------
GET /bikes     | Yes          | Returns all operational bikes being managed by the system operator
GET /regions   | No           | Returns all operational regions visible to the client

### GET /bikes
The following fields are all attributes within each "bike" object for this endpoint.

Field Name       | Required    | Defines
-----------------| ------------| ----------
latitude         | Yes         | The latitude of the bike. The field value must be a valid WGS 84 latitude in decimal degrees format. See: http://en.wikipedia.org/wiki/World_Geodetic_System, https://en.wikipedia.org/wiki/Decimal_degrees
longitude        | Yes         | The longitude of the bike. The field value must be a valid WGS 84 longitude in decimal degrees format. See: http://en.wikipedia.org/wiki/World_Geodetic_System, https://en.wikipedia.org/wiki/Decimal_degrees
bike_type        | Yes         | Key identifying the type of bike this is (e.g. "manual", "electric")

Example:

```json
{
  "data": [
    {
      "id": "bike_X7Q5TILR2JHG7",
      "type": "bikes",
      "attributes": {
        "latitude": 37.1231,
        "longitude": -135.12314,
        "bike_type": "manual"
      }
    },
    {
      "id": "bike_N3BXXM2RMUACI",
      "type": "bikes",
      "attributes": {
        "latitude": 37.1359,
        "longitude": -135.145,
        "bike_type": "electric"
      }
    }
  ]
}
```

### GET /regions
The following fields are all attributes within each "region" object for this endpoint.

Field Name       | Required    | Defines
-----------------| ------------| ----------
name             | Yes         | Public name for this region
polygon          | Yes         | Array of latitude/longitude coordinates that represent points of the geographical polygon area of the region
\- latitude      | Yes         | The latitude of the polygon point. The field value must be a valid WGS 84 latitude in decimal degrees format. See: http://en.wikipedia.org/wiki/World_Geodetic_System, https://en.wikipedia.org/wiki/Decimal_degrees
\- longitude     | Yes         | The field value must be a valid WGS 84 longitude in decimal degrees format. See: http://en.wikipedia.org/wiki/World_Geodetic_System, https://en.wikipedia.org/wiki/Decimal_degrees

Example:

```json
{
  "data": [
    {
      "id": "region_HFMHX2RF6GFQK",
      "type": "regions",
      "attributes": {
        "name": "Washington D.C.",
        "polygon": [
          {
            "latitude": 39.169748,
            "longitude": -76.6288024
          },
          {
            "latitude": 39.2805122,
            "longitude": -77.1746124
          },
          {
            "latitude": 38.796774,
            "longitude": -77.134666
          },
          {
            "latitude": 39.0502674,
            "longitude": -76.5504382
          }
        ]
      }
    }
  ]
}
```

