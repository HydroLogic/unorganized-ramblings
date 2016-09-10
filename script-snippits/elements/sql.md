
* Pull out losses
```sql
SELECT PropertyID, 
ExternalLongitude/1000000. LON, 
ExternalLatitude/1000000. LAT, 
AAL_STR.GU GU_STR,
AAL_STR.GU/1000000. GU_STR_LC, 
AAL_CON.GU GU_CON, 
AAL_CON.GU/1000000. GU_CON_LC,
AAL_TE.GU GU_TE,
AAL_TE.GU/1000000. GU_TE_LC 
FROM 
(SELECT * FROM IF_EXP_RDFL_MRS_OpenAddr.dbo.Property WHERE PortfolioID = 1) Prop 
LEFT JOIN 
(SELECT * FROM IF_RES_RDFL_MRS_OpenAddr.dbo.pml WHERE PMLLevelType = 'USERDEFINED3' AND AnalysisRunID = 1 
AND RefID%100 = 0) AAL_STR 
ON (AAL_STR.RefID)/100 = Prop.PropertyID 
LEFT JOIN 
(SELECT * FROM IF_RES_RDFL_MRS_OpenAddr.dbo.pml WHERE PMLLevelType = 'USERDEFINED3' AND AnalysisRunID = 1 
AND RefID%100 = 2) AAL_CON 
ON (AAL_CON.RefID)/100 = Prop.PropertyID 
LEFT JOIN 
(SELECT * FROM IF_RES_RDFL_MRS_OpenAddr.dbo.pml WHERE PMLLevelType = 'USERDEFINED3' AND AnalysisRunID = 1 
AND RefID%100 = 4) AAL_TE 
ON (AAL_TE.RefID)/100 = Prop.PropertyID
order by PropertyID
```

* Cell ID
```sql
create table aal.surge_aug2016_aal_iod_cellid as 
select round((((CASE WHEN "Long"::numeric < 0 THEN "Long"::numeric + 360.0 END)*1000.0)*1000000.0 ),0)+round((("Lat"::numeric+90.0)*1000.0),0) cellid, * from aal.surge_aug2016_aal_iod
```





