



* Join points to DFIRM in PostGIS
```sql
create table client_lafb.lafb_ty17_locationdata_la_dfirm as 
SELECT "SiteID", "Latitude", "Longitude", fld_ar_id, fld_zone, bfe, pt.geom
  FROM client_lafb.lafb_ty17_locationdata_raw pt 
  left join (select * from dfirm_haz_area.fld_haz_ar_la) pol
on st_dwithin(pt.geom, pol.geom, 0)
```




