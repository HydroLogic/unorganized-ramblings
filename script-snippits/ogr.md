

* Translate shapefile to geojson
    ogr2ogr -f GeoJSON -t_srs crs:84 [name].geojson [name].shp

    for %f in (*.shp) do ogr2ogr -f GeoJSON %~nf.geojson %f 

* Import file into a unique schema
    ogr2ogr -f PostgreSQL PG:"user=postgres dbname=tmp" anchorage.vrt -lco schema=openaddr_temp -nln ak_anchorage -lco GEOMETRY_NAME=geom -skipfailures -progress

for %f in (*.shp) do ogr2ogr -f PostgreSQL PG:"user=postgres dbname=tmp" %f -lco schema=scenario_2016_8_hermine -nln %~nf_shape -lco GEOMETRY_NAME=geom -skipfailures -progress


