# Data Source Publication IDs Report by Group of Authors

## COLUMN HEADERS: 
Elements Group ID|Elements User ID|Name|Position|Department|School|Employee ID|Net ID|Elements Publication ID|Date Published|DOI|ISSN|Document Sub-Type|Document Type|Scopus Article ID|Web of Science Article ID|Crossref DOI|PubMed Article ID|Europe PubMed Central Article ID

## REPORT EXAMPLE 

| Elements Group ID | Elements User ID | Name     | Position            | Department | School             | Employee ID | Net ID | Elements Publication ID | Date Published | DOI                 | ISSN      | Document Sub-Type                      | Document Type   | Scopus Article ID  | Web of Science Article ID | Crossref DOI                 | PubMed Article ID | Europe PubMed Central Article ID |
|-------------------|------------------|----------|---------------------|------------|--------------------|-------------|--------|-------------------------|----------------|---------------------|-----------|----------------------------------------|-----------------|--------------------|---------------------------|------------------------------|-------------------|----------------------------------|
| 15                | 255              | John Doe | Professor           | Medicine   | School of Medicine | 12345       | jad222 | 30270                   | 20160201       | 10.1038/ajg.2016.13 | 0002-9270 | Editorial Material, Editorial, Journal | Journal Article | 2-s2.0-84962296723 | WOS:000375399500019       | 10.1038/ajg.2016.13          | 26882945          | MED:26882945                     |
| 15                | 225              | John Doe | Professor           | Medicine   | School of Medicine | 12345       | jad222 | 23451                   | 20140222       | 10.9392/0000        | 0002-2345 | Journal Article                        | Journal Article | 2-s2.0-84942605114 | WOS:000369592700010       | 10.1097/MOG.0000000000000187 | 26039725          | MED:26039725                     |
| 15                | 12               | Jane Doe | Assistant Professor | Surgery    | School of Medicine | 23456       | jdd123 | 23451                   | 20140222       | 10.9392/0000        | 0002-2345 | Journal Article                        | Journal Article | 2-s2.0-84942605114 | WOS:000369592700010       | 10.1097/MOG.0000000000000187 | 26039725          | MED:26039725                     |
| 15                | 12               | Jane Doe | Assistant Professor | Surgery    | School of Medicine | 23456       | jdd123 | 89076                   | 20150901       | 10.1111/nmo.12613   | 1350-1925 | Article, Journal                       | Journal Article | 2-s2.0-84939774880 | WOS:000364747900005       | 10.1111/nmo.12613            | 26088614          | MED:26088614                     |
| 15                | 12               | Jane Doe | Assistant Professor | Surgery    | School of Medicine | 23456       | jdd123 | 12345                   | 20150901       | 10.1111/apt.13324   | 0269-2813 | Review, Journal                        | Journal Article | 2-s2.0-84939562887 | WOS:000359855900002       | 10.1111/apt.13324            | 26177572          | MED:26177572                     |

## NOTES: 

1. There are duplicates in this report. When two or more faculty members co-author a publication, the publication will be listed for each faculty member. 
2. There are occasionally two Scopus Article IDs for an article in the Reporting database. The first ID is usually for the "Article in Press" and the second ID is usually for the "Published version". This report overwrites the first Scopus Article ID so that only the second Scopus Article ID is displayed in the report. 

## WHEN TO USE THIS REPORT

Use this report when you need a list of unique Article Identfiers (from Scopus, Web of Science, Crossref, PubMed) as they are available in Elements for each faculty member's publications. Faculty members need to be part of a primary or auto group in Elements. 


## CODE BASE:
~~~sql
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
				--,pr.[Publication ID] 'pr Pub ID' 
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

		WHERE g.[name] = 'group name'
			--g.[name] = 'Digiestive Health Center'
			--g.[id] = 15
			--AND p.[publication-date] > YYYYMMDD AND p.[publication-date] <= YYYYMMDD
			--AND p.[publication-date] > 20200101 AND p.[publication-date] <= 20200131
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
	