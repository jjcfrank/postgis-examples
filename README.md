# PostGIS command examples for learning and testing
Below is a list of querries for PostGIS. You can test them [here](http://3.138.138.81) - a geospatial database using PgAdmin. To access it, use the credentials below:

user = user@user.com  |  password = user1234

## List of PostGIS querries

1a. List all zones and a count of pixel values based on the LiDAR land-cover raster

``` SQL
SELECT DISTINCT aux3.zonename, aux3.value, "nyc-lc-legend".type_lc, count(aux3.count)

FROM
(SELECT (aux1).*, aux2.zonename

FROM
(SELECT DISTINCT b.zonename, ST_ValueCount(a.rast) as aux1
FROM raster."nyc-lc" a, vector.nyc_zones b 
WHERE ST_Intersects(a.rast, b.wkt)
) as aux2
) as aux3

JOIN aspatial."nyc-lc-legend" ON aux3.value = "nyc-lc-legend".pix_val

GROUP BY aux3.zonename, aux3.value, "nyc-lc-legend".type_lc
ORDER BY 2 ASC, 3 DESC, 4 DESC
;
```

![](examples/postgis-example-1.gif)

1b. Similarly, this could be filtered by land-cover type using WHERE

``` SQL
SELECT DISTINCT aux3.zonename, aux3.value, "nyc-lc-legend".type_lc, count(aux3.count)

FROM
(SELECT (aux1).*, aux2.zonename

FROM
(SELECT DISTINCT b.zonename, ST_ValueCount(a.rast) as aux1
FROM raster."nyc-lc" a, vector.nyc_zones b 
WHERE ST_Intersects(a.rast, b.wkt)
) as aux2
) as aux3

JOIN aspatial."nyc-lc-legend" ON aux3.value = "nyc-lc-legend".pix_val

WHERE "nyc-lc-legend".type_lc = 'Tree Canopy'

GROUP BY aux3.zonename, aux3.value, "nyc-lc-legend".type_lc
ORDER BY 2 ASC, 3 DESC, 4 DESC
;

```

## License & copyright

© Frank Jimenez

Licensed under the [MIT Licence](LICENSE).