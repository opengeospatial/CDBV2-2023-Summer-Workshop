# OGC CDB 2.0 - Part 2: GeoPackage Data Store

This repository contains the draft OGC CDB 2.0 - Part 2: GeoPackage Data Store standard, as well as supporting information.

The latest draft candidate standard can be accessed [here in HTML](https://github.com/opengeospatial/CDBV2-2023-Summer-Workshop/releases/download/v0.1/23-XXX.html) and [here in PDF](https://github.com/opengeospatial/CDBV2-2023-Summer-Workshop/releases/download/v0.1/23-XXX.pdf).

## Overview

This standard defines a data store implementing the [CDB 2.0 Core](https://portal.ogc.org/files/104639) Conceptual Model based on the [OGC GeoPackage](https://www.geopackage.org/spec131/index.html) data container
and GeoPackage extensions for storing different data types.

The OGC CDB 2.0 standards define a conceptual model and data stores for encoding different types of data, including detailed 3D environments,
according to that conceptual model with a focus on deterministic performance and scalability.
The CDB standards enable a variety of use cases ranging from mobile computing at the edge to
large scale data repositories, including training, simulation, digital twins and acting as an efficient backing data store for OGC API implementations.

This standard is organized into multiple requirements classes, of which only _GeoPackage Data Store_ is mandatory.

## Profiles

This standard is organized in a modular way so as to make it easy to profile it for a particular use case, such as
a data repository or mobile computing at the edge.
An application profile of this standard can depend on the mandatory _GeoPackage Data Store_ requirements class
as well as additional specific requirements class specifying the use of a particular data organization, tiling grid
or encoding for specific data types, and can optionally introduce additional restrictions.
The following table illustrates some example profiles that could be defined based on this standard.

| Profile            | Requirement classes   | Additional restrictions
| -------            | -------------------   | -----------------------
| Repository         | GeoPackage Data Store <br /> LoD-Grouped GeoPackages <br /> GNOSIS Global Grid <br /> GeoTIFF Coverage Data <br /> Tiled Vector Data <br /> WKB Collection Tiled Vector Data <br /> Referenced 3D Models <br /> Referenced Shared 3D Models <br /> Referenced Unique 3D Models <br /> OpenFlight 3D Models <br /> JPEG-XL Imagery | Grouping tiles by 6 LoD <br /> Packaging data layers separately |
| Simulation         | GeoPackage Data Store <br /> LoD-Grouped GeoPackages <br /> GNOSIS Global Grid <br /> PNG Coverage Data <br /> Tiled Vector Data <br /> WKB Collection Tiled Vector Data <br /> Referenced 3D Models <br /> Referenced Shared 3D Models <br /> Integrated Unique 3D Model Tiles <br /> glTF 3D Models <br /> JPEG-XL Imagery | |
| Mobile             | GeoPackage Data Store <br /> Single GeoPackage Data Store <br /> GNOSIS Global Grid <br /> PNG Coverage Data <br /> Untiled Vector Data <br /> Referenced Shared 3D Models <br /> Integrated Unique 3D Model Tiles <br /> glTF 3D Models <br /> JPEG Imagery | |
| Compatibility      | GeoPackage Data Store <br /> LoD-Grouped GeoPackages <br /> CDB 1 Global Grid <br /> PNG Coverage Data <br /> Tiled Vector Data <br /> WKB Collection Tiled Vector Data <br /> Referenced 3D Models <br /> Referenced Shared 3D Models <br /> Referenced Unique 3D Models <br /> OpenFlight 3D Models <br /> JPEG-XL Imagery | |
| Compatibility+glTF | GeoPackage Data Store <br /> LoD-Grouped GeoPackages <br /> CDB 1 Global Grid <br /> PNG Coverage Data <br /> Tiled Vector Data <br /> WKB Collection Tiled Vector Data <br /> Referenced 3D Models <br /> Referenced Shared 3D Models <br /> Referenced Unique 3D Models <br /> glTF 3D Models <br /> JPEG-XL Imagery | |

## Requirements Classes

### Data store organization

#### GeoPackage Data Store

This class defines the mandatory requirements that must be satisfied by all data stores conforming to this CDB 2.0 GeoPackage Data Store standard.

These requirements specify:
- an encoding for datasets structured according to the _OGC CDB 2.0 - Part 1: Core_ conceptual model,
- the use of _OGC GeoPackage_ as the container to store the data,
- how to store metadata required by the Core conceptual model in GeoPackages,
- use of the _deterministic hierarchical tiles grouping & indexing extension for GeoPackages_, allowing to support different use cases from
  mobile computing at the edge to large scale data repositories, by configuring how data is stored in a single GeoPackage file, or across
  multiple GeoPackages split by data layer and or tiles and
- use of the _2D Tile matrix Set extension for GeoPackages_, providing support for variable width tile matrix sets in GeoPackages.

#### Single GeoPackage Data Store

This requirements class specifies that the entire CDB data store consists of a single GeoPackage file.

#### Single GeoPackage Data Layers

This requirements class specifies that individual data layers of the CDB data store consists of a single GeoPackage file,
meaning that none of them groups tiles by level of details in multiple GeoPackages.

#### LoD-grouped GeoPackages

This requirements class specifies that all data layers of the CDB data store group tiles
per level of details.
If the data store includes any vector data, this requirement class implies the _Tiled Vector Data_ requirements class.

### Tile Matrix Sets

#### GNOSIS Global Grid

This requirements class specifies the use of the _GNOSIS Global Grid_ tile matrix set for all tiled data layers.
This requirements class is mutually exclusive with the CDB 1 Global Grid requirements class, and any other Tile Matrix Set.

#### CDB 1 Global Grid

This requirements class specifies the use of the _CDB 1 Global Grid_ tile matrix set for all tiled data layers.
This requirements class is mutually exclusive with the GNOSIS Global Grid requirements class, and any other Tile Matrix Set.

### Imagery

#### JPEG Imagery

This requirements class specifies the use of JPEG as a format for encoding tiled imagery.
This requirements class is mutually exclusive with the JPEG-XL Imagery Data requirements class, and any other imagery encoding.

#### JPEG-XL Imagery

This requirements class specifies the use of JPEG-XL as a format for encoding tiled imagery.
This requirements class is mutually exclusive with the JPEG Imagery Data requirements class, and any other imagery encoding.

### Coverage Data

#### GeoTIFF Coverage Data

This requirements class specifies the use of the _OGC Gridded Coverage Extension_ for GeoPackages for storing coverage data, such as digital elevation models,
using the OGC GeoTIFF encoding.
This requirements class is mutually exclusive with the PNG Coverage Data requirements class, and any other coverage encoding.

#### PNG Coverage Data

This requirements class specifies the use of the _OGC Gridded Coverage Extension_ for GeoPackages for storing coverage data, such as digital elevation models,
using the PNG encoding.
This requirements class is mutually exclusive with the GeoTIFF Coverage Data requirements class, and any other coverage encoding.

### Vector Data

#### Untiled WKB Vector Data

This requirements class specifies the use of the Well-known binary encoding for vector data as defined in the Core GeoPackage standard for storing vector data,
without using tiles.
This requirements class is mutually exclusive with the Tiled Vector Data.

#### Tiled Vector Data

This requirements class specifies the use of the _Tiled vector data extension for GeoPackages_ for vector data, including use of the
associated attributes table requirements class.
This requirements class is mutually exclusive with the Untiled Vector Data.

##### WKB Collection Tiled Vector Data

This requirements class specifies the use of a Well-known binary encoding for tiled vector data as the payload for these tiles.
This requirements class depends on Tiled Vector Data and is mutually exclusive with any other vector data encoding.

### 3D Models

#### Referenced 3D Models

This requirements class specifies the use of vector reference points positioning, and optionally orienting and scaling, 3D models (whether instanced/shared or unique).

##### Referenced Shared 3D Models

This requirements class specifies the use of the referenced 3D models requirements class for storing unique 3D models that are instantiated at multiple geospatial locations, such as a typical tree or generic house (traditionally called "geo-typical").
This requirements class depends on "Referenced 3D Models" and is mutually exclusive with the _Integrated Shared 3D Model Tiles_ requirements class.

##### Referenced Unique 3D Models

This requirements class specifies the use of the referenced 3D models requirements class for storing unique 3D models that are instantiated only once at a single geospatial location, such as a landmark, large building, or building of a particular interest for a simulation (traditionally called "geo-specific").
This requirements class is mutually exclusive with the _Integrated Unique 3D Model Tiles_ requirements class.

#### Integrated 3D Model Tiles

This requirements class specifies the use of a single integrated payload batching all models within a tile (whether only shared models, unique models, or both).
If supported by the 3D model encoding, each model has a dedicated hierarchical node with its own transformation information, allowing to easily reposition it relative to the terrain model in use.

##### Integrated Unique 3D Model Tiles

This requirements class specifies the use of a single integrated payload batching all unique models within a tile.
This requirements class depends on "Integrated 3D Model Tiles" and is mutually exclusive with the _Referenced Unique 3D Models_ requirements class.
If supported by the 3D model encoding, each model has a dedicated hierarchical node with its own transformation information, allowing to easily reposition it relative to the terrain model in use.

##### Integrated Shared 3D Model Tiles

This requirements class specifies the use of a single integrated payload batching all shared models within a tile.
This requirements class is mutually exclusive with the _Referenced Shared 3D Models_ requirements class.
If supported by the 3D model encoding, each model has a dedicated hierarchical node with its own transformation information, allowing to easily reposition it relative to the terrain model in use.
If supported by the 3D model encoding, multiple instance of the same 3D model within the tile re-use the same 3D mesh information.

#### glTF 3D Models

This requirements class specifies the use of glTF as a format for encoding 3D models.
This requirements class is mutually exclusive with the OpenFlight 3D Models requirements class, and any other 3D models encoding.

#### OpenFlight 3D Models

This requirements class specifies the use of OpenFlight as a format for encoding 3D models.
This requirements class is mutually exclusive with the glTF 3D Models requirements class, and any other 3D models encoding.

## Sample Datasets

(work in progress to update to latest version)

[Different dataset configurations produced for CDB X Tech Sprint](https://github.com/sofwerx/cdb2-eng-report/blob/master/11-tiling-coverages.adoc#san-diego-cdb-layers-packaged-together)

[Single 9.8 GiB (compressed) GeoPackage of San Diego](https://data.ogc.org/2020/11/SanDiego.gpkg.7z)

( glTF 3D Models, PNG Elevation, JPEG Imagery, Mapbox Vector Tiles )

[Multiple Geopackages 9.9 GiB (compressed) of San Diego](https://data.ogc.org/2020/11/SanDiego.7z)

( glTF 3D Models, PNG Elevation, JPEG Imagery, Mapbox Vector Tiles, grouped by 5 tile levels )

[Multiple Geopackages 217.5 MiB (compressed) of San Diego (subset)](https://portal.ogc.org/files/?artifact_id=95370)

## Producer and Reader implementations

[Overview of tools implementing OGC CDB 2.0 - Part 2: GeoPackage Data Store standard](implementations/README.adoc)

## Communication

Join the [SWG mailing list (Private)](https://lists.ogc.org/mailman/listinfo/cdb.swg).

Most of all work on the specification takes place in [GitHub issues](https://github.com/opengeospatial/CDBV2-2023-Summer-Workshop/issues),
so browse there to get a good idea of what is happening, as well as past decisions.

## Additional information

The draft **OGC CDB 2.0 - Part 2: GeoPackage Data Store standard** references the [**OGC CDB 2.0 - Part 1: Core**](https://portal.ogc.org/files/104639), [OGC GeoPackage](https://www.geopackage.org/spec131/index.html), and multiple GeoPackage extensions.

## [Contributing](../CONTRIBUTING.md)

The contributor understands that any contributions, if accepted by the OGC Membership, shall be incorporated into OGC Standards documents and that all copyright and intellectual property shall be vested to the OGC.

The CDB Standards Working Group (SWG) is the group at OGC responsible for the stewardship of the standard, but is working to do as much work in public as possible.

* [CDB Standards Working Group Charter](https://portal.ogc.org/?m=projects&a=view&project_id=466)
* [Open issues](https://github.com/opengeospatial/CDBV2-2023-Summer-Workshop/issues)
* [Copy of License Language](../LICENSE)

Pull Requests from contributors are welcomed. However, please note that by sending a Pull Request or Commit to this GitHub repository, you are agreeing to the terms in the Observer Agreement https://portal.ogc.org/files/?artifact_id=92169
