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
* For CDB 1.x:
   * $ Significant Size Lower Bound > { 111319 \over 2^{LOD+11} } $
* For CDB 2.0:
   * $ Significant Size Lower Bound > { 111319 \over 2^{LOD+4} } $

The actual equation to obtain the value of 111319 meters per degree at the equator is $ L = {a \times \pi \times 180 deg} $.


| CDB 2 Level | CDB 1.x Level | Significant Size |
| ----------- | ------------- | ---------------- |
|  0 | -7 | SS > 6957.4m   |
|  1 | -6 | SS > 3478.7m   |
|  2 | -5 | SS > 1739.36m  |
|  3 | -4 | SS > 869.68m   |
|  4 | -3 | SS > 434.84m   |
|  5 | -2 | SS > 217.42m   |
|  6 | -1 | SS > 108.71m   |
|  7 |  0 | SS > 54.35498m |
|  8 |  1 | SS > 27.17749m |
|  9 |  2 | SS > 13.58875m |
| 10 |  3 | SS > 6.794373m |
| 11 |  4 | SS > 3.397186m |
| 12 |  5 | SS > 1.698593m |
| 13 |  6 | SS > 0.849296m |
| 14 |  7 | SS > 0.424648m |
| 15 |  8 | SS > 0.212324m |
| 16 |  9 | SS > 0.106162m |
| 17 | 10 | SS > 0.053081m |

> **_NOTE:_** At each successive level, the significant size (geometric error) is cut in half.

> **_NOTE:_** The significant size values in this table come from the LOD levels in Table 3-1 from Volume 1 of CDB 1.x
