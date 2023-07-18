# Vector Data

## Open Questions


-----------------
## Decisions

### Significant Size (Geometric Error) Table

> TODO: Define Significant Size

Given a model or 2D polygonal vector with a significant size or geometric error, to store that feature in CDB requires calculation of the zoom level or level of detail (LOD) that the feature should be present within.  This calculated level is a function of the 2D TMS level's pixel size for a raster.  Using the following functions, one can construct a table that represents what size features are present at a given zoom level or level of detail.

The formula for calculating the significant size per level is constructed from the WGS84 ellipsoid's major semi-axis, the size of the tile, and the number of raster pixels of that tile.

First, calculate the length of a degree in meters at the equator, "L":

$$ L = a \times {\pi \over 180 } \approx 111,319.4 $$

where "a" is the length of the major semi-axis of the WGS-84 ellipsoid, namely 6378137.0m; "a" is also known as the equatorial radius.

Next, use that value to calculate the lower bound of the significant size for a tile:

$$ SSLowerBound \gt {L \times TileWidthInDegrees \over 2^{level+1} * TileWidthInPixels} $$

So a given zoom level or level of detail will contain features with a geometric error larger than the next finest level's pixel spacing.

### CDB 1.x Significant Size

For CDB 1.x tiling, the tile size at the equator is 1 degree, and the number of pixels per tile at level 0 is 1024.  LODs can be as low as -10, and the formula works for these as well, since the pixels per tile is halved and the size does not change:

$$ SSLowerBound > { L \times 1 \over 2^{LOD+1} \times 1024 } $$

This function simplifies to this:

$$ SSLowerBound > { L \over 2^{LOD+11} } $$

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

### CDB 2.0 Significant Size

For CDB 2.0 tiling, the significant size equation works out to be:

$$ SSLowerBound > { L \times 90 \over 2^{LOD+1} \times 256 } $$

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

> **_NOTE:_** At each successive level, the significant size (geometric error) is cut in half.
