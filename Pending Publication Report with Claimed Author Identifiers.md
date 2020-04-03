# PENDING PUBLICATION REPORT WITH CLAIMED AUTHOR IDENTIFIERS

## COLUMN HEADERS: 

User ID|Name|Role|Department|School|Pending Book|Pending Chapter|Pending Conference|Pending Journal Article| No Pending (Null)|Pending Other|Pending Poster|Pending Presentation| Pending Report|Pending Scholarly Edition|Pending Software/Code| User ID| Is Claimed? | Researcher ID|ORCID| Scopus Author ID|Email


## REPORT EXAMPLE

| User ID | Name     | Role      | Department | School                      | Pending Book | Pending Chapter | Pending Conference | Pending Journal Article | No Pending (Null) | Pending Other | Pending Poster | Pending Presentation | Pending Report | Pending Scholarly Edition | Pending Software/Code | User ID | Is Claimed? | Researcher ID | ORCID               | Scopus Author ID                    | Email                     |
|---------|----------|-----------|------------|-----------------------------|--------------|-----------------|--------------------|-------------------------|-------------------|---------------|----------------|----------------------|----------------|---------------------------|-----------------------|---------|-------------|---------------|---------------------|-------------------------------------|---------------------------|
| 1       | John Doe | Professor | Medicine   | Feinberg School of Medicine | 12           | 1               | 2                  | 52                      | 0                 | 0             | 2              | 1                    | 3              | 1                         | 2                     | 1       | 1           | A-6866-2019   | 0000-0001-7302-7587 | 57211486598;7403534728              | john.doe@northwestern.edu |
| 2       | Jane Doe | Professor | Surgery    | Feinberg School of Medicine | 1            | NULL            | NULL               | NULL                    | NULL              | NULL          | NULL           | NULL                 | NULL           | 1                         | 3                     | 2       | 1           | W-8145-2019   | 0000-0003-2790-1143 | 35276463900;57202040844;57206608563 | jane.doe@northwestern.edu |


## NOTES: 
1. There are two UserID columns. These should match each other. You can delete the second one.
2. There is an "Is Claimed?" column that you can delete.  
3. Multiple Researcher IDs, Scopus Author IDs, Emails, are separated by a semi-colon. 
3. This report runs in Microsoft SQL Serv 2012 and 2014. 
4. This report works with the Elements Reporting Database for: 5.19.0.2603 release build. 

## WHEN TO USE THIS REPORT:
Use this report when you need a list of pending publications by user and also want to check if the user has claimed unique identifiers. 


## CODE BASE:
~~~sql

use [Elements-reporting2]


SELECT * FROM	(
	SELECT * FROM (

		SELECT  u.[ID],u.[Computed Name Abbreviated],  u.[Position] 
				 ,LEFT(u.[Department],  CHARINDEX(';', u.[Department])) "True Department" 
				 ,RIGHT(u.[Department], LEN(u.[Department]) - CHARINDEX(';', u.[Department])) "True School"
				 ,p.[Type] AS pend_pub_type
				 ,COUNT(p.ID) AS "pend_pub_count"
		
	

		FROM    [User] u 
         			LEFT JOIN [Pending Publication] pp ON pp.[User ID] = u.ID 
        			LEFT JOIN [Publication] p ON p.[ID] = pp.[Publication ID]
			
			
		WHERE u.[Is Current Staff] = 1 
				 AND u.[Is Academic] = 1
				-- AND u.[ID] = '8886'

		GROUP BY u.[ID]
				 ,[Computed Name Abbreviated] 
				 ,[Position]
				 ,u.[Department] 
				 ,p.[Type]
			

	) t

	PIVOT
	(
		MAX(pend_pub_count) 
		FOR [pend_pub_type] IN 
			([Book], 
			[Chapter], 
			[Conference], 
			[Journal Article], 
			[Null], 
			[Other], 
			[Poster], 
			[Presentation], 
			[Report], 
			[Scholarly edition], 
			[Software / Code])
	) AS pend_pivot_table

) as final_pend_pivot_table

LEFT JOIN
				
				(SELECT * FROM	(
					SELECT
						 [User ID]
						,[Is Claimed]
						,[Identifier Scheme ID]
						 ,STUFF(
							 (SELECT ';' + [Identifier Value] from [User Identifier]
							  WHERE [User ID] = a.[User ID] AND [Identifier Scheme ID] = a.[Identifier Scheme ID] AND a.[Is Claimed] = 1
							  FOR XML PATH (''))
							  , 1, 1, '')  AS ME_String
					FROM [User Identifier] AS a
					WHERE a.[Is Claimed] = 1 AND a.[User ID] = '8886'
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
		 
 	ON final_pend_pivot_table.[ID] = final_id_pivot_table2.[User ID]

