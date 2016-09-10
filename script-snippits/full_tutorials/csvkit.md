```bash
# csvkit is to tabular data what the standard Unix text processing suite
# (grep, sed, cut, sort) is to text. As such, csvkit adheres to the Unix
# philosophy.
#
# [Getting started](http://csvkit.readthedocs.org/en/latest/tutorial/getting_started.html)

# ================
# Install csvkit =
# ================

$ pip install csvkit

# ===================
# get some csv data =
# ===================

$ wget -O 2009.csv -U "Mozilla/5.0 (X11; U; Linux x86_64; en-US) AppleWebKit/534.16 (KHTML, like Gecko) Chrome/10.0.648.205 Safari/534.16" http://www.data.gov/download/4029/csv
$ wget -O 2010.csv -U "Mozilla/5.0 (X11; U; Linux x86_64; en-US) AppleWebKit/534.16 (KHTML, like Gecko) Chrome/10.0.648.205 Safari/534.16" http://www.data.gov/download/4509/csv

# Use head to check the first five lines:
$ head -n 5 2009.csv
$ head -n 5 2010.csv

# As you can see, `2010.csv` has multiple header lines.
# We need to modify this file to fix the issue, but as a matter of
# best practice let's backup our originals first:
$ cp 2009.csv 2009_original.csv && cp 2010.csv 2010_original.csv

# Select lines 1 and 2 of `2010_original.csv`and (d)elete them
$ cat 2010_original.csv | sed "1,2d" > 2010.csv

# ==================================
## Cutting up the data with csvcut =
# ==================================

# List columns. [-n] displays column numbers in output.
$ csvcut -n 2009.csv
>  1: State Name
>  2: State Abbreviate
>  3: Code
>  4: Montgomery GI Bill-Active Duty
>  5: Montgomery GI Bill- Selective Reserve
>  6: Dependents' Educational Assistance
>  7: Reserve Educational Assistance Program
>  8: Post-Vietnam Era Veteran's Educational Assistance Program
>  9: TOTAL
> 10:

# Grab columns 2 and 3. [-c] column indices or names to be extracted.
$ csvcut -c 2,3 2009.csv
> State Name,Code
> AL,01
> AK,02
> AZ,04
> AR,05

# Flipping column order with csvcut
$ csvcut -c 9,1 2009.csv | head -n 5
> TOTAL,State Name
> 12426,ALABAMA
> 1158,ALASKA
> 33986,ARIZONA
> 5513,ARKANSAS

# ===================================
# Statistics on demand with csvstat =
# ===================================

$ csvcut -c 1,4,9,10 2009.csv | csvstat
> 1. State Name
>   <type 'unicode'>
>   Nulls: Yes
>   Unique values: 53
>   Max length: 17
>   Samples: "FLORIDA", "IDAHO", "ARIZONA", "OHIO", "IOWA"
> 2. Montgomery GI Bill-Active Duty
>   <type 'int'>
>   Nulls: Yes
>   Min: 435
>   Max: 34942
>   Mean: 6263
>   Median: 3548.0
>   Unique values: 53
> 3. TOTAL
>   <type 'int'>
>   Nulls: Yes
>   Min: 768
>   Max: 46897
>   Mean: 9748
>   Median: 6520.0
>   Unique values: 53
> 4.
>       Empty column

# =================================
# Searching for rows with csvgrep =
# =================================

# Find states that begin w/ the letter "I"

$ csvcut -c 1,"TOTAL" 2009.csv | csvgrep -c 1 -r "^I"
> State Name,TOTAL
> ILLINOIS,"21,964"

# ======================
# Sorting with csvsort =
# ======================

# Sort the rows by the first column. [-r] sort in descending order.

$ csvcut -c 9,1 2009.csv | csvsort -r | head -n 5
> TOTAL,State Name
> 46897,CALIFORNIA
> 40402,TEXAS
> 36394,FLORIDA
> 33986,ARIZONA

# ======================================
# Using line numbers as proxy for rank =
# ======================================

# The -l flag is a special flag that can be passed to any csvkit utility
# in order to add a column of line numbers to its output. Since this data
# is being sorted we can use those line numbers as a proxy for rank:

$ csvcut -c 9,1 2009.csv | csvsort -r -l | head -n 11
> line_number,TOTAL,State Name
> 1,46897,CALIFORNIA
> 2,40402,TEXAS
> 3,36394,FLORIDA
> 4,33986,ARIZONA
> 5,21964,ILLINOIS
> 6,20541,VIRGINIA
> 7,18236,GEORGIA
> 8,15730,NORTH CAROLINA
> 9,13967,NEW YORK
> 10,13962,MISSOURI

# Missouri had the tenth largest population of individuals claiming veterans
# education benefits.

# ===================================
# Reading through data with csvlook =
# ===================================

# Display the data in a fixed-width table

$ csvcut -c 9,1 2009.csv | csvsort -r -l | csvlook
> ---------------------------------------------
> |  line_number | TOTAL | State Name         |
> ---------------------------------------------
> |  1           | 46897 | CALIFORNIA         |
> |  2           | 40402 | TEXAS              |
> |  3           | 36394 | FLORIDA            |
> |  4           | 33986 | ARIZONA            |
> |  5           | 21964 | ILLINOIS           |
> |  6           | 20541 | VIRGINIA           |
> |  7           | 18236 | GEORGIA            |
> |  8           | 15730 | NORTH CAROLINA     |
> |  9           | 13967 | NEW YORK           |
> |  10          | 13962 | MISSOURI           |
> [...]

# ==================
# Saving your work =
# ==================

# The complete ranking might be a useful thing to have around. Rather than
# computing it every time, let's use output redirection to save a copy of it:

$ csvcut -c 9,1 2009.csv | csvsort -r -l > 2009_ranking.csv

# ================================================
# Converts various tabular data formats into CSV =
# ================================================

# Convert an Excel .xls file:
$ in2csv test.xls

# Standardize the formatting of a CSV file (quoting, line endings, etc.):
$ in2csv examples/realdata/FY09_EDU_Recipients_by_State.csv

# Fetch csvkit's open issues from the Github API, convert the JSON response into a CSV and write it to a file:
$ curl https://api.github.com/repos/onyxfish/csvkit/issues?state=open | in2csv -f json -v > issues.csv

# Convert a DBase DBF file to an equivalent CSV:
$ in2csv examples/testdbf.dbf > testdbf_converted.csv

# Fetch the ten most recent robberies in Oakland, convert the GeoJSON
# response into a CSV and write it to a file:
$ curl "http://oakland.crimespotting.org/crime-data?format=json&type=robbery&count=10" \
    | in2csv -f geojson > robberies.csv

# ============================================
# Cleans a CSV file of common syntax errors. =
# ============================================

# Cleans a CSV file of common syntax errors

$ csvclean -n examples/bad.csv
> Line 3: Expected 3 columns, found 4 columns
> Line 4: Expected 3 columns, found 2 columns

# ===============================================================
# Converts a CSV file into JSON or GeoJSON (depending on flags) =
# ===============================================================

# Convert veteran's education dataset to JSON keyed by state abbreviation:
$ csvjson -k "State Abbreviate" -i 4 examples/realdata/FY09_EDU_Recipients_by_State.csv
> {
>     [...]
>     "WA":
>     {
>         "": "",
>          "Code": "53",
>          "Reserve Educational Assistance Program": "549",
>          "Dependents' Educational Assistance": "2,192",
>          "Montgomery GI Bill-Active Duty": "7,969",
>          "State Name": "WASHINGTON",
>          "Montgomery GI Bill- Selective Reserve": "769",
>          "State Abbreviate": "WA",
>          "Post-Vietnam Era Veteran's Educational Assistance Program": "13",
>          "TOTAL": "11,492"
>     },
>     [...]
> }

# Converting locations of public art into GeoJSON:
$ csvjson --lat latitude --lon longitude --k slug --crs EPSG:4269 -i 4 examples/test_geo.csv
> {
>     "type": "FeatureCollection",
>     "bbox": [
>         -95.334619,
>         32.299076986939205,
>         -95.250699,
>         32.351434
>     ],
>     "crs": {
>         "type": "name",
>         "properties": {
>             "name": "EPSG:4269"
>         }
>     },
>     "features": [
>         {
>             "geometry": {
>                 "type": "Point",
>                 "coordinates": [
>                     -95.30181,
>                     32.35066
>                 ]
>             },
>             "type": "Feature",
>             "id": "dcl",
>             "properties": {
>                 "photo_credit": "",
>                 "description": "In addition to being the only coffee shop in downtown Tyler, DCL also features regular exhibitions of work by local artists.",
>                 "artist": "",
>                 "title": "Downtown Coffee Lounge",
>                 "install_date": "",
>                 "address": "200 West Erwin Street",
>                 "last_seen_date": "3/30/12",
>                 "type": "Gallery",
>                 "photo_url": ""
>             }
>         },
>     [...]
>     ]
> }

# ========================================
# Generate SQL statements for a CSV file =
# ========================================

# - access
# - sybase
# - sqlite
# - informix
# - firebird
# - mysql
# - oracle
# - maxdb
# - postgresql
# - mssql

# Generate SQL statements for a CSV file or execute those statements
# directly on a database. In the latter case supports both creating
# tables and inserting data:

# Generate a statement in the PostgreSQL dialect:
$ csvsql -i postgresql  examples/realdata/FY09_EDU_Recipients_by_State.csv

# Create a table and import data from the CSV directly into Postgres:
$ createdb test
$ csvsql --db postgresql:///test --table fy09 --insert examples/realdata/FY09_EDU_Recipients_by_State.csv

# For large tables it may not be practical to process the entire table.
# One solution to this is to analyze a sample of the table. In this case it
# can be useful to turn off length limits and null checks with the
# `no-constraints` option:
$ head -n 20 examples/realdata/FY09_EDU_Recipients_by_State.csv | csvsql --no-constraints --table fy09

# Create tables for an entire folder of CSVs and import data from those files directly into Postgres:
$ createdb test
$ csvsql --db postgresql:///test --insert examples/*.csv
```
