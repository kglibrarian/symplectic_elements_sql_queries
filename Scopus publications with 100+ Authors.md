# SCOPUS PUBLICATIONS WITH 100+ AUTHORS

## COLUMN HEADERS: 
Elements Group ID|Elements User ID|Name|Role|Department|School|Employee ID|NetID|Is Current Staff? (1-yes)|Is Academic? (1-yes)|Elements Publication ID|Publication Date|Authors|Title|Source|Issue|Name of Conference|Begin Page|End Page|Volume|DOI|ISSN|Document SubType|Document Type|Author Count|First Data Source|Preferred Record ID|Scopus ID|Web of Science|Cross Ref DOI|PubMed ID|Europe PubMed ID


## CODE BASE:
~~~sql
use [Elements-reporting2]
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
				,p.[authors]
				,p.[Computed Title]
				,p.[Canonical journal title]
				,p.[issue]
				,p.[name-of-conference]
				,p.[pagination Begin]
				,p.[pagination End]
				,p.[volume]
				,p.[doi]
				,p.[ISSN]
				,p.[Types]
				,p.[Type]
				,p.[Author Count]
				--,pr.[Publication ID] 'pr Pub ID'
				,p.[Data Source] 'Top Data Source' 
				,upp.[Preferred Record ID] 
				,pr.[Data Source]
				,pr.[Data Source Proprietary ID]
				
		FROM [Group] g
			LEFT JOIN [Group User Membership] gum
			ON g.[ID] = gum.[Group ID]
			LEFT JOIN [Publication User Relationship] pur
			ON gum.[User ID] = pur.[User ID]
			LEFT JOIN [User Publication Preferences] upp 
			ON pur.[User ID] = upp.[User ID] AND pur.[Publication ID] = upp.[Publication ID]
			LEFT JOIN [Publication] p
			ON pur.[Publication ID] = p.[ID]
			LEFT JOIN [Publication Record] pr
			ON pur.[Publication ID] = pr.[Publication ID]
			LEFT JOIN [User] u
			ON gum.[User ID] = u.[ID]

		WHERE g.[id] = 15
				AND u.[Is Current Staff] = 1
				AND u.[Is Academic] = 1
				AND p.[Author Count] >= 100
				--AND  p.[Data Source] = 'Scopus'
				--AND upp.[Preferred Record ID] is NULL
 
			--g.[name] = 'Research to Prevent Blindness'
			--AND p.[publication-date] >= 20150101 AND p.[publication-date] <= 20191231
			--g.[name] = 'Digiestive Health Center'
			--g.[id] = 15
			--AND p.[publication-date] >= YYYYMMDD AND p.[publication-date] <= YYYYMMDD
			--AND p.[publication-date] >= 20200101 AND p.[publication-date] <= 20200131
			--AND p.[ISSN] IN ('1538-3598', '2574-3805', '2380-6591' ) 
			--AND u.[Is Current Staff] = 1
			--AND u.[Is Academic] = 1
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
	