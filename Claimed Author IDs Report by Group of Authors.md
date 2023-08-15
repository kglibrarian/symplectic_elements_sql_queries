# Claimed Author IDs Report by Group of Authors

## COLUMN HEADERS:
Elements Group ID|Elements User ID|Elements User ID|Name|Position|Department|School|Employee ID|Net ID|Elements User ID	Any Claimed IDs? 1 - yes, NULL -no|Claimed WoS Researchers ID|Claimed ORCID|Claimed Scopus Author ID|Claimed Email 


## REPORT EXAMPLE: 

|Elements Group ID|Elements User ID|Elements User ID|Name          |Position |Department                          |School                      |Employee ID|Net ID|Elements User ID|Any Claimed IDs? 1 - yes, NULL -no|Claimed WoS Researchers ID|Claimed ORCID      |Claimed Scopus Author ID|Claimed Email     |
|-----------------|----------------|----------------|--------------|---------|------------------------------------|----------------------------|-----------|------|----------------|----------------------------------|--------------------------|-------------------|------------------------|------------------|
|173              |255             |255             |John Doe      |Professor|Medicine, Gastroenterology Division;| Feinberg School of Medicine|1002064    |jd234 |255             |1                                 |NULL                      |0000-1234-8260-1250|1232556261              |johndoe@nwu.edu   |
|173              |518             |518             |Jane Doe      |Professor|Medicine, Hepatology Division;      | Feinberg School of Medicine|1003916    |ja123 |518             |1                                 |NULL                      |NULL               |3454142194              |NULL              |




## CODE BASE:
~~~sql
use [Elements-reporting2]


SELECT * FROM	(
	
		SELECT  gum.[Group ID]
				,gum.[User ID]
				,u.[ID]
				,u.[Computed Name Abbreviated]
				,u.[Position] 
				 ,LEFT(u.[Department],  CHARINDEX(';', u.[Department])) "True Department" 
				 ,RIGHT(u.[Department], LEN(u.[Department]) - CHARINDEX(';', u.[Department])) "True School"
				,u.[Proprietary ID]
				,u.[Username]	
	

		FROM    [Group] g
			LEFT JOIN [Group User Membership] gum
			ON g.[ID] = gum.[Group ID]
			LEFT JOIN [User] u 
			ON gum.[User ID] = u.[ID]
         			
			
		WHERE  g.[name] = 'group name'
                -- g.[name] = 'Digestive Health Center'
				-- g.[id] = '114'
				-- AND u.[Is Current Staff] = 1 
				-- AND u.[Is Academic] = 1
				-- AND u.[ID] = '8886'

		GROUP BY u.[ID]
				 ,[Computed Name Abbreviated] 
				 ,[Position]
				 ,u.[Department] 
				 ,u.[Proprietary ID]
				 ,u.[Username]
				 ,gum.[User ID]
				 ,gum.[Group ID]
			

	) t

LEFT JOIN
				
				(SELECT * FROM	(
					SELECT
						 [User ID]
						,[Is Claimed]
						,[Identifier Scheme ID]
						 ,STUFF(
							 (SELECT ';' + [Identifier Value] from [User Identifier]
							  WHERE [User ID] = a.[User ID] AND 
							  [Is Claimed] = 1 AND
							  [Identifier Scheme ID] = a.[Identifier Scheme ID]  
							  FOR XML PATH (''))
							  , 1, 1, '')  AS ME_String
					FROM [User Identifier] AS a
					WHERE a.[Is Claimed] = 1 
						--AND a.[User ID] = '8886'
					GROUP BY [User ID]
							,[Is Claimed]
							,[Identifier Scheme ID]
				) q
		
			PIVOT
				(
				MAX([ME_String]) 
				FOR [Identifier Scheme ID] IN 
					([8], 
					[9], 
					[10], 
					[17])
		
				)
			
		 AS final_id_pivot_table
		 ) as final_id_pivot_table2
		 
 	ON t.[ID] = final_id_pivot_table2.[User ID]
