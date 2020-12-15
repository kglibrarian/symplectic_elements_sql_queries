# Data Source Publication IDs Report by Group of Authors

## COLUMN HEADERS: 
Elements Group ID|Elements User ID|Name|Position|Department|School|Employee ID|Net ID|Elements Publication ID|Date Published|DOI|ISSN|Document Sub-Type|Document Type|Scopus Article ID|Web of Science Article ID|Crossref DOI|PubMed Article ID|Europe PubMed Central Article ID|Elements Pub ID|NIHMS ID|PMCID|PMID|PubMed ID 
## CODE BASE:
~~~sql

use [Elements-reporting2]

SELECT a.*, b.*
FROM

(
					SELECT * FROM (
							SELECT gum.[Group ID]
									,gum.[User ID]
									--,pur.[User ID]
									--,pur.[Publication ID] 'Pur Pub ID'
									,u.[Computed Name Abbreviated] AS 'Name'
									,u.[Position] 
									 ,LEFT(u.[Department],  CHARINDEX(';', u.[Department])) AS 'Department' 
									 ,RIGHT(u.[Department], LEN(u.[Department]) - CHARINDEX(';', u.[Department])) AS 'School'
									,u.[Proprietary ID] AS 'Employee ID'
									,u.[Username] AS 'Net ID'
									,u.[Is Current Staff]
									,u.[Is Academic]
									,p.[ID] AS 'Publication ID'
									,p.[Publication-Date]
									,p.[doi]
									,p.[ISSN]
									,p.[Types]
									,p.[Type]
									,pr.[Publication ID] 'pr Pub ID' 
									,pr.[Data Source]
									,pr.[Data Source Proprietary ID]
									
					
							FROM [Group] g
								LEFT JOIN [Group User Membership] gum
								ON g.[ID] = gum.[Group ID]
								LEFT JOIN [Publication User Relationship] pur
								ON gum.[User ID] = pur.[User ID]
								LEFT JOIN [Publication] p
								ON pur.[Publication ID] = p.[ID]
								LEFT JOIN [Publication Record] pr
								ON pur.[Publication ID] = pr.[Publication ID]
								LEFT JOIN [User] u
								ON gum.[User ID] = u.[ID]

							WHERE g.[id] = 173
								--g.[name] = 'Digestive Health Center'
								--g.[id] = 15
								--AND p.[publication-date] >= YYYYMMDD AND p.[publication-date] <= YYYYMMDD
								-- AND p.[publication-date] >= 20200101 AND p.[publication-date] <= 20201231
								--AND p.[ISSN] IN ('1538-3598', '2574-3805', '2380-6591' ) 
								-- AND u.[Is Current Staff] = 1
								-- AND u.[Is Academic] = 1
								--AND p.[types] IN ('Article', 'Article in Press', 'Article, Early Access, Journal', 'Article, Journal','Book', 'Case Reports, case-report', 'Chapter', 'Journal', 'Journal Article', 'Letter, Journal', 'Note, Article, Journal', 'Note, Journal', 'other, Journal Article', 'Practice Guideline, Journal Article', 'research-article, Journal Article', 'Review, Book Series', 'Review, Journal', 'review-article, Journal Article', 'review-article, Review, Journal Article', 'Short Survey, Journal')

						
							) AS t

							PIVOT 
								(
									MAX([Data Source Proprietary ID])
									FOR [Data Source] IN
										([Scopus],
										 [Web of Science],
										 [Crossref],
										 [PubMed], 
										 [Europe PubMed Central])
		


								) AS pub_id_pivot_table

) a

LEFT JOIN

(			
			SELECT max([Publication ID]) as 'Max PR Pub ID', 
					max([NIHMS]) as 'Max PR NIHMS', 
					max([PMCID]) as 'Max PR PMCID', 
					max([PMID]) as 'Max PR PMID', 
					max([PubMed]) as 'Max PR PubMed'
			FROM
	
				(SELECT DISTINCT ID, [Publication ID], -- xmlname,
				  (CASE
					WHEN xmlname.value('/Names[1]/name[1]','varchar(100)') LIKE '%nihms%' THEN xmlname.value('/Names[1]/name[1]','varchar(100)') 
					WHEN xmlname.value('/Names[1]/name[2]','varchar(100)') LIKE '%nihms%' THEN xmlname.value('/Names[1]/name[2]','varchar(100)') 
					WHEN xmlname.value('/Names[1]/name[3]','varchar(100)') LIKE '%nihms%' THEN xmlname.value('/Names[1]/name[3]','varchar(100)') 
					ELSE NULL
				  END) AS NIHMS,
				  (CASE 
					WHEN xmlname.value('/Names[1]/name[1]','varchar(100)') LIKE '%PMC%' THEN xmlname.value('/Names[1]/name[1]','varchar(100)') 
					WHEN xmlname.value('/Names[1]/name[2]','varchar(100)') LIKE '%PMC%' THEN xmlname.value('/Names[1]/name[2]','varchar(100)') 
					WHEN xmlname.value('/Names[1]/name[3]','varchar(100)') LIKE '%PMC%' THEN xmlname.value('/Names[1]/name[3]','varchar(100)') 
					ELSE NULL
				   END ) AS PMCID,
				  (CASE 
					WHEN xmlname.value('/Names[1]/name[1]','varchar(100)') LIKE '%PMID%' THEN xmlname.value('/Names[1]/name[1]','varchar(100)') 
					WHEN xmlname.value('/Names[1]/name[2]','varchar(100)') LIKE '%PMID%' THEN xmlname.value('/Names[1]/name[2]','varchar(100)') 
					WHEN xmlname.value('/Names[1]/name[3]','varchar(100)') LIKE '%PMID%' THEN xmlname.value('/Names[1]/name[3]','varchar(100)') 
					ELSE NULL
				   END ) AS PMID,
					(CASE 
					WHEN xmlname.value('/Names[1]/name[1]','varchar(100)') LIKE '%pubmed%' THEN xmlname.value('/Names[1]/name[1]','varchar(100)') 
					WHEN xmlname.value('/Names[1]/name[2]','varchar(100)') LIKE '%pubmed%' THEN xmlname.value('/Names[1]/name[2]','varchar(100)') 
					WHEN xmlname.value('/Names[1]/name[3]','varchar(100)') LIKE '%pubmed%' THEN xmlname.value('/Names[1]/name[3]','varchar(100)') 
					ELSE NULL
				   END ) AS PubMed
	 
	  
				  FROM 
						(SELECT ID, 
							[Publication ID],
							[external-identifiers],
							CONVERT(XML,'<Names><name>'  
							+ REPLACE([external-identifiers],',','</name><name>') + '</name></Names>') AS xmlname
						 FROM [Publication Record] AS PR
						-- WHERE pr.[Publication ID] = 15411
						) c
			) AS d
		
			GROUP BY d.[Publication ID]

	) b


ON a.[Publication ID] = b.[Max PR Pub ID]
