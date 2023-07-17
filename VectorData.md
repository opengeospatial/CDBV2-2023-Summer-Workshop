# Vector Data

## Open Questions


-----------------
## Decisions

### Significant Size (Geometric Error) Table

How to calculate a model's or vector's significant size:
* Calculate the model's or vector's geometric error
* Find the first significant size in the table that is the same or smaller in this table
* Put the model or geometry at this LOD level in the CDB

The formula for the lower bound is based on the approximate length in meters of an arc of one degree at the equator:

| CDB 1.x Formula | CDB 2.x Formula |
| --------------- | --------------- |
| $$ Lower Bound > { 111319 \over 2^{LOD+11} } $$ | $$ Lower Bound > { 111319 \over 2^{LOD+4} } $$ |

The actual equation to obtain the value of 111319 meters per degree at the equator is:

$$ L = {a \times \pi \times 180 deg} $$

where "a" is the length of the major semi-axis of the WGS-84 ellipsoid; "a" is also known as the equatorial radius.


| CDB 2 Level | Equiv CDB 1.x Level | Significant Size | 
| ----------- | ------------- | ---------------- |
|  0 | -9 | SS > 19567.9m   |
|  1 | -8 |  19567.9m >= SS > 9783.9m    |
|  2 | -7 |   9783.9m >= SS > 4891.97m   |
|  3 | -6 |  4891.97m >= SS > 2445.98m   |
|  4 | -5 |  2445.98m >= SS > 1222.99m   |
|  5 | -4 |  1222.99m >= SS > 611.496m   |
|  6 | -3 |  611.496m >= SS > 305.748m   |
|  7 | -2 |  305.748m >= SS > 152.874m   |
|  8 | -1 |  152.874m >= SS >  76.437m   |
|  9 |  0 |   76.437m >= SS >  38.218m   |
| 10 |  1 |   38.218m >= SS >  19.109m   |
| 11 |  2 |   19.109m >= SS >   9.5546m  |
| 12 |  3 |   9.5546m >= SS >   4.7773m  |
| 13 |  4 |   4.7773m >= SS >   2.3887m  |
| 14 |  5 |   2.3887m >= SS >   1.1943m  |
| 15 |  6 |   1.1943m >= SS >   0.59716m |
| 16 |  7 |  0.59716m >= SS >   0.29858m |
| 17 |  8 |  0.29858m >= SS >   0.14929m |
| 18 |  9 |  0.14959m >= SS >   0.07464m |
| 19 | 10 |  0.07464m >= SS >   0.03732m |

| CDB 1.x Level | Significant Size |
| ------------- | ---------------- |
| -10 | SS > 55659.5m |
| -9 |  55659.5m > SS > 27829.6m  |
| -8 |  27829.6m > SS > 13914.8m  |
| -7 |  13914.8m > SS > 6957.4m   |
| -6 |   6957.4m > SS > 3478.7m   |
| -5 |   3478.7m > SS > 1739.36m  |
| -4 |  1739.36m > SS > 869.68m   |
| -3 |   869.68m > SS > 434.84m   |
| -2 |   434.84m > SS > 217.42m   |
| -1 |   217.42m > SS > 108.71m   |
|  0 |   108.71m > SS > 54.35498m |
|  1 | 54.35498m > SS > 27.17749m |
|  2 | 27.17749m > SS > 13.58875m |
|  3 | 13.58875m > SS > 6.794373m |
|  4 | 6.794373m > SS > 3.397186m |
|  5 | 3.397186m > SS > 1.698593m |
|  6 | 1.698593m > SS > 0.849296m |
|  7 | 0.849296m > SS > 0.424648m |
|  8 | 0.424648m > SS > 0.212324m |
|  9 | 0.212324m > SS > 0.106162m |
| 10 | 0.106162m > SS > 0.053081m |

> **_NOTE:_** At each successive level, the significant size (geometric error) is cut in half.

> **_NOTE:_** The significant size values in this table come from the LOD levels in Table 3-1 from Volume 1 of CDB 1.x
