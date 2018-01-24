# Dockless Bikeshare Feed Specification (DBFS)
This document explains the types of files and data that comprise the Dockless Bikeshare Feed Specification (DBFS) and defines the fields used in all of those files.

## Table of Contents

* [Introduction](#introduction)
* [Authentication and Authorization](#authentication-and-authorization)
* [API Response Format](#api-response-format)
* [Endpoints](#endpoints)
    * [GET /bikes](#get-bikes)

## Introduction
This specification has been designed with the following concepts in mind:

* Provide the status of the system at this moment
* Do not provide information whose primary purpose is historical

Historical data, including station details and ride data is to be provided by a more compact specification designed specifically for such archival purposes. The data in the specification contained in this document is intended for consumption by clients intending to provide real-time (or semi-real-time) transit advice and is designed as such.

## Authentication and Authorization
System operator will provide each client with an API key secret. On each request to one of our endpoints, clients will need to supply their given secret as a bearer token in the HTTP Authorization request header.

System operators and clients will also agree geofences representing the operational region(s) that the client is authorized to oversee. Only bikes physically located within the agreed upon geofences will be available via this API.

## API Response Format
Endpoints should use the JSON API version 1.0 format for responses (see http://jsonapi.org/format/1.0/).

## Endpoints
This specification defines the following endpoints along with their associated content:

Endpoint     | Required     | Defines
------------ | ------------ | ----------
/bikes       | Yes          | Returns all operational bikes being managed by the system operator

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
      "id": "bike_OIBB44VCZMOFANBK3",
      "type": "bikes",
      "attributes": {
        "latitude": 37.1231,
        "longitude": -135.12314,
        "bike_type": "manual"
      }
    },
    {
      "id": "bike_2QXJI6ZNHLGW2OAVJ",
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

