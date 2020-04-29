# Claimed Author IDs by group membership

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
         			
			
		WHERE  g.[name] = 'Digestive Health Center'
				-- AND u.[Is Current Staff] = 1 
				 --AND u.[Is Academic] = 1
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
