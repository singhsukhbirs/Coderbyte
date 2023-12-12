# Coderbyte
# <B> change variable name coordingly </B>
<H3> Sql Yearly Regional vendors</H3>
SELECT cur.Year
        , cur.Region
        , cur.UniqueVendors
        , CASE
          WHEN prev.PrevUniqueVendors IS NULL THEN "N/A"
          ELSE (
            CONCAT(ROUND(((cur.UniqueVendors - prev.PrevUniqueVendors)
               / prev.PrevUniqueVendors) * 100, 2), '%')
          )
        END as PercentageChangeFromLastYear


FROM (
  SELECT 
    Year
    , Region
    , COUNT(DISTINCT VendorID) as UniqueVendors
  FROM maintable_671KZ
  GROUP BY Year, Region
) cur

LEFT JOIN (
  SELECT 
    Year AS PrevYear
    , Region As PrevRegion
    , COUNT(DISTINCT VendorID) as PrevUniqueVendors
  FROM maintable_671KZ
  GROUP BY PrevYear, PrevRegion
) prev ON cur.Region = prev.PrevRegion AND cur.Year = prev.PrevYear + 1


ORDER BY cur.Year ASC
 , cur.UniqueVendors DESC
 , cur.Region ASC

;
