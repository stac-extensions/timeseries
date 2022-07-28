# Time Series Extension Specification

- **Title:** Time Series
- **Identifier:** <https://stac-extensions.github.io/timeseries/v1.0.0/schema.json>
- **Field Name Prefix:** ts
- **Scope:** Item, Collection
- **Extension [Maturity Classification](https://github.com/radiantearth/stac-spec/tree/master/extensions/README.md#extension-maturity):** Proposal
- **Owner**: @emmanuelmathot

This document explains the Time Series Extension to the [SpatioTemporal Asset Catalog](https://github.com/radiantearth/stac-spec) (STAC) specification.
STAC item representing a temporal series dataset should use this extension to provide additional information about the time series. Typical use cases include:

- [MODIS Vegetation Index Products](https://modis.gsfc.nasa.gov/data/dataprod/mod13.php)  
- Water Levels (e.g. [Copernicus Global Land Service Water Level](https://land.copernicus.eu/global/products/wl))

- Examples:
  - [Item example](examples/item.json): Shows the basic usage of the extension in a STAC Item
  - [Collection example](examples/collection.json): Shows the basic usage of the extension in a STAC Collection
- [JSON Schema](json-schema/schema.json)
- [Changelog](./CHANGELOG.md)

## Item Properties and Collection Fields

| Field Name    | Type      | Description                                                                                                                                                                    |
| ------------- | --------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| ts:period     | string    | string representing a recurring time interval with repeating rules                                                                                                             |
| ts:dates      | \[string] | List of the dates represented in the time series datasets. It is formatted as date-time according to [RFC 3339, section 5.6](https://tools.ietf.org/html/rfc3339#section-5.6). |
| ts:parameters | \[string] | List of parameters or variables in the time series                                                                                                                             |

One of `ts:period` or `ts:dates` is **REQUIRED**.

### Additional Field Information

There are 2 main date representations in the time series extension:

- **Continuous time series**: The time series is continuous and the dates are represented by a recurring time interval with repeating rules in the field [`ts:period`](#tsperiod). 
- **Discrete time series**: The time series is discrete and the dates are represented by a list of dates in the field [`ts:dates`](#tsdates).

In both cases, it is also recommended that period covered by the time series is defined by the first and last date of the time series using the [Date and Time Range](https://github.com/radiantearth/stac-spec/blob/master/item-spec/common-metadata.md#date-and-time-range) fields

#### ts:period

This is the duration of the time series. It is formatted as [repeating intervals](https://staging.standards.calconnect.org/csd/cc-18012.html#toc18). They are formed by adding `R[n]/` to the beginning of an interval expression, where `R` is used as the letter itself and `[n]` is replaced by the number of repetitions. Leaving out the value for `[n]` or specifying a value of `-1`, means an unbounded number of repetitions.

Some examples:

- `R12/20150929T140000/F2W` means every 2 weeks for 12 times starting on `2015-09-29T14:00:00Z`

## Relation types

This extension extends the Link Object used in all STAC entities (Catalogs, Collections, Items). It requires specific relation types to be set for the `rel` field in the Link Object.

### Coverage

Links to a [Coverage JSON](https://covjson.org) document.

| Field Name | Type   | Description                                                                            |
| ---------- | ------ | -------------------------------------------------------------------------------------- |
| rel        | string | **REQUIRED**. Must be set to `coverage`.                                               |
| href       | string | **REQUIRED**. Link to the Coverage JSON document.                                      |
| type       | string | **REQUIRED**. Type of the coverage (e.g `application/prs.coverage+json`).              |
| ts:api     | string | If applicable, the code of the API producing the document (See below for more details) |

#### Coverage JSON APIs

| API code         | Description                                                                                                                            |
| ---------------- | -------------------------------------------------------------------------------------------------------------------------------------- |
| wcs              | [WCS2.0 getCoverage request](https://docs.geoserver.org/stable/en/user/community/cov-json/index.html#example-wcs-2-0-timeseries) model |
| ogcapi-coverages | OGC API [Coverages](https://github.com/opengeospatial/ogcapi-coverages)                                                                |
| egms             | [European Ground Motion Service](https://land.copernicus.eu/pan-european/european-ground-motion-service) API                           |


## Contributing

All contributions are subject to the
[STAC Specification Code of Conduct](https://github.com/radiantearth/stac-spec/blob/master/CODE_OF_CONDUCT.md).
For contributions, please follow the
[STAC specification contributing guide](https://github.com/radiantearth/stac-spec/blob/master/CONTRIBUTING.md) Instructions
for running tests are copied here for convenience.

### Running tests

The same checks that run as checks on PR's are part of the repository and can be run locally to verify that changes are valid. 
To run tests locally, you'll need `npm`, which is a standard part of any [node.js installation](https://nodejs.org/en/download/).

First you'll need to install everything with npm once. Just navigate to the root of this repository and on 
your command line run:
```bash
npm install
```

Then to check markdown formatting and test the examples against the JSON schema, you can run:
```bash
npm test
```

This will spit out the same texts that you see online, and you can then go and fix your markdown or examples.

If the tests reveal formatting problems with the examples, you can fix them with:
```bash
npm run format-examples
```
