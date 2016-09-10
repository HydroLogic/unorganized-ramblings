

## gdal_polygonize 

* Simple shapefile creation from raster
    gdal_polygonize.py myraster.tif -f "ESRI Shapefile" mylayer.shp
    for %f in (*.tif) do gdal_polygonize %f -f "ESRI Shapefile" %~nf.shp

* [Import polygon directly into Postgis](http://gis.stackexchange.com/a/29673/62608)
    gdal_polygonize.py myraster.tif -f PostgreSQL PG:"dbname='postgis' user='postgres'" mylayer
    for %f in (*.tif) do gdal_polygonize %f -f PostgreSQL PG:"dbname='tmp' user='postgres'" %~nf.shp


## gdal_calc

* Create a binary raster for easy polygon creation
    ```
    for %f in (*.tif) do gdal_calc -A %f --outfile=out/%~nf.tif --calc="(A*0+1)*(A>0)" --type="Int16" --co=compress=lzw --NoDataValue=-9999 --overwrite

* [Take the maximum of three rasters](http://gis.stackexchange.com/a/148171/62608)
    ```
    gdal_calc -A fluvial_matt.tif -B pluvial_hojjat.tif -C surge_jeong.tif --outfile=result_calc_max.tif --calc="maximum(maximum(A,B),C)"
    ```

# Scratch

for %f in (*.tif) do gdal_polygonize %f -f "ESRI Shapefile" %~nf.shp
