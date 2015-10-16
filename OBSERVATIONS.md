# Observations

This file contains observations and insights into the raw data, which was imported into a relational database for querying.

<hr>

Two agencies are included in `emissions_by_agency_by_vehicle_type.csv`
 but not in `emissions_by_agency.csv`.

agyAbbrev	| agyType	| agyName
--- | --- | ---
ABMC	| Civilian	| American Battle Monuments Commission
USAID	| Civilian	| US Agency for International Development

```` sql
SELECT
  DISTINCT
    av.agyAbbrev
    ,av.agyType
    ,av.agyName
FROM emissions_by_agency a
RIGHT JOIN emissions_by_agency_by_vehicle_type av ON av.agyAbbrev = a.agyAbbrev
WHERE a.agyId IS null
````

<hr>

Each agency has between 1-4 different types of vehicles. All agencies have gas vehicles.

vehicle_types	| agency_count
--- | ---
Diesel, E85, Electric, Gas|	22
Diesel, E85, Gas|	12
E85, Gas	|10
Diesel, Gas	| 3
Gas	| 2


```` sql
SELECT
 vehicle_types
 ,count(DISTINCT agency_abbrev) AS agency_count
FROM (
     SELECT
      dataYear AS year_effective
      ,agyType AS agency_type
      ,agyParentName AS parent_agency_name
      ,agyAbbrev AS agency_abbrev
      ,agyName AS agency_name
      ,count(DISTINCT vehType) AS vtype_count
      ,group_concat(DISTINCT vehType ORDER BY vehType SEPARATOR ', ') AS vehicle_types
    FROM emissions_by_agency_by_vehicle_type
    GROUP BY 1,2,3,4,5
    ORDER BY 6 DESC
) zz
GROUP BY 1
````

<hr>

Aggregations in `emissions_by_agency.csv` are reliable.

```` sql
SELECT
  a.agyId AS agency_id
  ,a.agyAbbrev AS agency_abbrev
  ,a.agyName AS agency_name
  -- emissions
  ,a.gge_e85
  ,a.gge_gas
  ,a.gge_other
  ,a.gge_e85 + a.gge_gas + a.gge_other AS gge_calculated_total
  ,a.gge_total
  -- vehicles
  ,a.vehcnt_e85
  ,a.vehcnt_gas
  ,a.vehcnt_other
  ,a.vehcnt_total
  ,a.vehcnt_e85 + a.vehcnt_gas + a.vehcnt_other AS vehicle_count_calculated_total
FROM emissions_by_agency a
HAVING (gge_calculated_total <> gge_total) OR (vehicle_count_calculated_total <> vehcnt_total)
-- >> zero rows
````
