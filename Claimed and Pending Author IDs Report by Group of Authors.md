use [Elements-reporting2]


SELECT * 
    FROM(
	
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
         			
			
            WHERE  g.[name] = 'Organisation'
                    --g.[name] = 'Digestive Health Center'
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
			

	) grouptable

	

LEFT JOIN
				
	(SELECT  [User ID]
             ,[8] as 'ClaimedWoS'
             ,[9] as 'ClaimedORCID'
             ,[10] as 'ClaimedScopus' 
             ,[17] as 'ClaimedEmail'
				FROM(
						SELECT [User ID]
							   ,[Is Claimed]
							   ,[Identifier Scheme ID] 
							   ,STUFF(
								    (SELECT ';' + [Identifier Value] 
                                        FROM [User Identifier]
								        WHERE [User ID] = a.[User ID] 
                                        AND [Is Claimed] = 1 
                                        AND [Identifier Scheme ID] = a.[Identifier Scheme ID]  
								    FOR XML PATH ('')), 1, 1, '')  AS Person_Claimed_ID_String
						FROM [User Identifier] AS a
						WHERE a.[Is Claimed] = 1 
							  --AND a.[User ID] = '8886'
						GROUP BY [User ID]
								,[Is Claimed]
								,[Identifier Scheme ID]
					) person_claimed_table
		
				    PIVOT
					    (MAX([Person_Claimed_ID_String]) 
					    FOR [Identifier Scheme ID] IN 
						    ([8], 
						    [9], 
						    [10], 
						    [17])
		
					) AS pivot_claimedtable
		 ) AS pivot_claimedtable_2
		 
 	ON grouptable.[ID] = pivot_claimedtable_2.[User ID]

    LEFT JOIN
				
	(SELECT  [User ID]
             ,[8] as 'PendingWoS'
             ,[9] as 'PendingORCID'
             ,[10] as 'PendingScopus' 
             ,[17] as 'PendingEmail'
             ,[23] as 'PendingSSRN'
				FROM(
						SELECT [User ID]
							   ,[Is Pending]
							   ,[Identifier Scheme ID] 
							   ,STUFF(
								    (SELECT ';' + [Identifier Value] 
                                        FROM [User Identifier]
								        WHERE [User ID] = a.[User ID] 
                                        AND [Is Pending] = 1 
                                        AND [Identifier Scheme ID] = a.[Identifier Scheme ID]  
								    FOR XML PATH ('')), 1, 1, '')  AS Person_Pending_ID_String
						FROM [User Identifier] AS a
						WHERE a.[Is Pending] = 1 
							  --AND a.[User ID] = '8886'
						GROUP BY [User ID]
								,[Is Pending]
								,[Identifier Scheme ID]
					) person_pending_table
		
				    PIVOT
					    (MAX([Person_Pending_ID_String]) 
					    FOR [Identifier Scheme ID] IN 
						    ([8], 
						    [9], 
						    [10], 
						    [17],
                            [23])
		
					) AS pivot_pendingtable
		 ) AS pivot_pendingtable_2
		 
 	ON grouptable.[ID] = pivot_pendingtable_2.[User ID]

     -- USER  IDENTIFER Table
     -- SELECT * FROM [Identifier Scheme]