# Vector Data

## Open Questions


-----------------
## Decisions

----
## Significant Size (Geometric Error) Table

> **_Requirement:_**  A 3D model or a polygonal vector feature **SHALL** be placed in a CDB's level of detail (or zoom level) at every level that has a pixel spacing that is smaller than the feature's significant size (or geometric error), if and only if that level exists in the CDB.<br>
> A linear vector feature **SHALL** be placed in a CDB's level of detail (or zoom level) at every level that has a pixel spacing that is smaller than the feature's width (which is the feature's significant size).

> _Note:_  The CDB's levels must extend high enough to include this feature

> **_Requirement:_**  If a 3D model has more than one representation or level of detail in its appearance, each refinement of the model is placed at the CDB level of detail (or zoom level) at which the significant size of the geometric change from the coarser model is larger than the significant size of the level.

> **_Requirement:_**  For a 2D geometric vector, the vector SHALL be simplified to reduce complexity at coarser CDB levels so that the change in the vector from level to level matches the significant size (or geometric error) of that level within the CDB.

### What is Significant Size

> TODO:  Define Significant Size

### How to Calculate Significant Size For a Feature

> TODO: Define How to calculate Significant Size

### How to Match a Significant Size to a CDB's Level

Given a model or 2D polygonal vector with a significant size or geometric error, to store that feature in CDB requires calculation of the zoom level or level of detail (LOD) that the feature should be present within.  This calculated level is a function of the 2D TMS level's pixel size for a raster.  Using the following functions, one can construct a table that represents what size features are present at a given zoom level or level of detail.

The formula for calculating the significant size per level is constructed from the WGS84 ellipsoid's major semi-axis, the size of the tile, and the number of raster pixels of that tile.

First, calculate the length of a degree in meters at the equator, "L":

$$ L = a \times {\pi \over 180 } \approx 111,319.5 $$

where "a" is the length of the major semi-axis of the WGS-84 ellipsoid, namely 6378137.0m; "a" is also known as the equatorial radius.

Next, use that value to calculate the lower bound of the significant size for a tile.  This is the meters per degree times the size of a tile's pixel in degrees times the ratio of the current tile's size compared to the next tile's size.

$$ SS \gt MetersPerDegree \times DegreesPerPixel \times MetersPerPixelRatioBetweenLevels $$

Or more precisely:

$$ SS \gt L \times {TileWidthInDegrees \over TileWidthInPixels} \times { NextLevelMetersPerPixel \over CurrentLevelMetersPerPixel} $$

This formula corresponds to the meters per pixel of the next highest/finest level in the 2D Tile Matrix Set.  So for a given zoom level, or level of detail, the specified level will contain features with a geometric error larger than the next finest level's pixel spacing.

> **_Requirement:_**  The significant size calculation for a 2D Tile Matrix Set SHALL be determined by the formula above.

---
### Assumptions

These formulas assume the following:
* The tiling used attempts to maintain a roughly square tile size as the tiles get closer to the poles.
* All tiles at a specified zoom level use the same significant size.

---
### CDB 1.x Significant Size

For CDB 1.x tiling, at level 0 the tile size at the equator is 1 degree, and the number of pixels per tile is 1024.  For successive positive LODs, the tile size is halved at each level.  For negative LODs, the tile size is fixed, but the pixel count per tile is halved at each successively decreasing level.

So the equation for positive LODs with a fixed pixel count and varying tile sizes works out to be:

$$ SS >  L \times { {1 \over 2^{LOD}} \over 1024 } \times {1 \over 2} $$

The equation for negative LODs with a changing pixel count changes and fixed tile size is:

$$ SS >  L \times {1 \over 2^{LOD + 10} } \times {1 \over 2} $$

These two functions simplify to the same equation:

$$ SS > { L \over 2^{LOD+11} } $$

This function is used for all tiles, whether near the equator or the poles, because CDB 1.x tiles change size to accomodate the longitude compression that naturally happens.  Below is Table 3-1 from CDB 1.x Volume 1, that is derived from this equation:

| CDB 1.x Level | Significant Size |
| :-----------: | :--------------- |
| -10 | SS > 55659.5m |
| -9 | SS > 27829.6m  |
| -8 | SS > 13914.8m  |
| -7 | SS > 6957.4m   |
| -6 | SS > 3478.7m   |
| -5 | SS > 1739.36m  |
| -4 | SS > 869.68m   |
| -3 | SS > 434.84m   |
| -2 | SS > 217.42m   |
| -1 | SS > 108.71m   |
|  0 | SS > 54.35498m |
|  1 | SS > 27.17749m |
|  2 | SS > 13.58875m |
|  3 | SS > 6.794373m |
|  4 | SS > 3.397186m |
|  5 | SS > 1.698593m |
|  6 | SS > 0.849296m |
|  7 | SS > 0.424648m |
|  8 | SS > 0.212324m |
|  9 | SS > 0.106162m |
| 10 | SS > 0.053081m |

> **_NOTE:_** This table can be expanded up to level 23.  To do so, halve the significant size (geometric error) value at each successive level.

To calculate the CDB 1.x level from the significant size, the following formula can be used:

$$ Level = ceil(log_2 ( {L \over SS} )) - 11 $$

---
### CDB 2.0 Significant Size (GNOSIS Global Grid)

For CDB 2.0 tiling (GNOSIS Global Grid), the tile size starts as 90 x 90 degrees at level 0 and halves at each successive level, and the pixels per tile is always 256.  So the significant size equation works out to be:

$$ SS > L \times { {90 \over 2^{LOD}} \over 256 } \times {1 \over 2} $$

Which simplifies to:

$$ SS > { 90L \over 2^{LOD+9} } $$

This function is used for all tiles, whether near the equator or the poles, because CDB 2.0 tiles change size to accomodate the longitude compression that naturally happens.  Below is the equivalent table, similar to the one from CDB 1.0:

| CDB 2 Level | Significant Size | Similar CDB 1.x Level |
| :---------: | :--------------- | :-------------------: |
|  0 |  SS > 19567.9m   | -9 |
|  1 |  SS > 9783.9m    | -8 |
|  2 |  SS > 4891.97m   | -7 |
|  3 |  SS > 2445.98m   | -6 |
|  4 |  SS > 1222.99m   | -5 |
|  5 |  SS > 611.496m   | -4 |
|  6 |  SS > 305.748m   | -3 |
|  7 |  SS > 152.874m   | -2 |
|  8 |  SS >  76.437m   | -1 |
|  9 |  SS >  38.218m   |  0 |
| 10 |  SS >  19.109m   |  1 |
| 11 |  SS >   9.5546m  |  2 |
| 12 |  SS >   4.7773m  |  3 |
| 13 |  SS >   2.3887m  |  4 |
| 14 |  SS >   1.1943m  |  5 |
| 15 |  SS >   0.59716m |  6 |
| 16 |  SS >   0.29858m |  7 |
| 17 |  SS >   0.14929m |  8 |
| 18 |  SS >   0.07464m |  9 |
| 19 |  SS >   0.03732m | 10 |

> **_NOTE:_** To expand this table to higher levels, halve the significant size (geometric error) value at each successive level.

To calculate the CDB 2 level from the significant size, the following formula can be used:

$$ Level = ceil(log_2 ( {90L \over SS} )) - 9 $$
