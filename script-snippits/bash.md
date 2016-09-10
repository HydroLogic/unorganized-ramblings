

* Loop recursively through files in a directory
    
    for /R %f in (*.tif) do gdal_calc
    

### CSVKit

* Look into a large flat file and display first X header

    csvcut -c 2,3 2009.csv | head -n 5

    
