# Identifying Secondary Appointments for Feinberg in Elements

## CODE BASE:
~~~sql
SELECT secondaryMED.[username] FROM (
	SELECT u.[username] FROM [User] as u WHERE u.[Is Current Staff] = '1' AND u.[Is Academic] = '1' AND u.[DeptClass1] LIKE '%med-%'
	UNION
	SELECT u.[username] FROM [User] as u WHERE u.[Is Current Staff] = '1' AND u.[Is Academic] = '1' AND u.[DeptGroup1] LIKE '%med-%' 
	UNION
	SELECT u.[username] FROM [User] as u WHERE  u.[Is Current Staff] = '1' AND u.[Is Academic] = '1'AND u.[PositionType1] LIKE '%med-%' 
	UNION
	SELECT u.[username] FROM [User] as u WHERE u.[Is Current Staff] = '1' AND u.[Is Academic] = '1' AND u.[PositionCat1] LIKE '%med-%'  
	UNION
	SELECT u.[username] FROM [User] as u WHERE u.[Is Current Staff] = '1' AND u.[Is Academic] = '1' AND u.[Dept2] LIKE '%med-%' 
	UNION
	SELECT u.[username] FROM [User] as u WHERE u.[Is Current Staff] = '1' AND u.[Is Academic] = '1' AND u.[DeptClass2] LIKE '%med-%' 
	UNION
	SELECT u.[username] FROM [User] as u WHERE u.[Is Current Staff] = '1' AND u.[Is Academic] = '1' AND u.[DeptGroup2] LIKE '%med-%'  
	UNION
	SELECT u.[username] FROM [User] as u WHERE u.[Is Current Staff] = '1' AND u.[Is Academic] = '1' AND u.[PositionType2] LIKE '%med-%' 
	UNION
	SELECT u.[username] FROM [User] as u WHERE u.[Is Current Staff] = '1' AND u.[Is Academic] = '1' AND  u.[PositionCat2] LIKE '%med-%' 
	UNION
	SELECT u.[username] FROM [User] as u WHERE u.[Is Current Staff] = '1' AND u.[Is Academic] = '1' AND u.[Dept3] LIKE '%med-%' 
	UNION
	SELECT u.[username] FROM [User] as u WHERE u.[Is Current Staff] = '1' AND u.[Is Academic] = '1' AND u.[DeptClass3] LIKE '%med-%' 
	UNION
	SELECT u.[username] FROM [User] as u WHERE u.[Is Current Staff] = '1' AND u.[Is Academic] = '1' AND u.[DeptGroup3] LIKE '%med-%' 
	UNION
	SELECT u.[username] FROM [User] as u WHERE u.[Is Current Staff] = '1' AND u.[Is Academic] = '1' AND u.[PositionType3] LIKE '%med-%' 
	UNION
	SELECT u.[username] FROM [User] as u WHERE u.[Is Current Staff] = '1' AND u.[Is Academic] = '1' AND u.[PositionCat3] LIKE '%med-%'  
	UNION
	SELECT u.[username] FROM [User] as u WHERE u.[Is Current Staff] = '1' AND u.[Is Academic] = '1' AND u.[Dept4] LIKE '%med-%' 
	UNION
	SELECT u.[username] FROM [User] as u WHERE u.[Is Current Staff] = '1' AND u.[Is Academic] = '1' AND u.[DeptClass4] LIKE '%med-%' 
	UNION
	SELECT u.[username] FROM [User] as u WHERE u.[Is Current Staff] = '1' AND u.[Is Academic] = '1' AND u.[DeptGroup4] LIKE '%med-%' 
	UNION
	SELECT u.[username] FROM [User] as u WHERE u.[Is Current Staff] = '1' AND u.[Is Academic] = '1' AND u.[PositionType4] LIKE '%med-%' 
	UNION
	SELECT u.[username] FROM [User] as u WHERE u.[Is Current Staff] = '1' AND u.[Is Academic] = '1' AND u.[PositionCat4] LIKE '%med-%'  
	UNION
	SELECT u.[username] FROM [User] as u WHERE u.[Is Current Staff] = '1' AND u.[Is Academic] = '1' AND u.[Dept5] LIKE '%med-%'  
	UNION
	SELECT u.[username] FROM [User] as u WHERE u.[Is Current Staff] = '1' AND u.[Is Academic] = '1' AND u.[DeptClass5] LIKE '%med-%' 
	UNION
	SELECT u.[username] FROM [User] as u WHERE u.[Is Current Staff] = '1' AND u.[Is Academic] = '1' AND u.[DeptGroup5] LIKE '%med-%'  
	UNION
	SELECT u.[username] FROM [User] as u WHERE u.[Is Current Staff] = '1' AND u.[Is Academic] = '1' AND u.[PositionType5] LIKE '%med-%'  
	UNION
	SELECT u.[username] FROM [User] as u WHERE u.[Is Current Staff] = '1' AND u.[Is Academic] = '1' AND u.[PositionCat5] LIKE '%med-%' 
	UNION
	SELECT u.[username] FROM [User] as u WHERE u.[Is Current Staff] = '1' AND u.[Is Academic] = '1' AND u.[Dept6] LIKE '%med-%'  
	UNION
	SELECT u.[username] FROM [User] as u WHERE u.[Is Current Staff] = '1' AND u.[Is Academic] = '1' AND u.[DeptClass6] LIKE '%med-%' 
	UNION
	SELECT u.[username] FROM [User] as u WHERE u.[Is Current Staff] = '1' AND u.[Is Academic] = '1' AND u.[DeptGroup6] LIKE '%med-%' 
	UNION
	SELECT u.[username] FROM [User] as u WHERE u.[Is Current Staff] = '1' AND u.[Is Academic] = '1' AND u.[PositionType6] LIKE '%med-%' 
	UNION
	SELECT u.[username] FROM [User] as u WHERE u.[Is Current Staff] = '1' AND u.[Is Academic] = '1' AND u.[PositionCat6] LIKE '%med-%' 
	UNION
	SELECT u.[username] FROM [User] as u WHERE u.[Is Current Staff] = '1' AND u.[Is Academic] = '1' AND u.[Dept7] LIKE '%med-%'  
	UNION
	SELECT u.[username] FROM [User] as u WHERE u.[Is Current Staff] = '1' AND u.[Is Academic] = '1' AND u.[DeptClass7] LIKE '%med-%'
	UNION
	SELECT u.[username] FROM [User] as u WHERE u.[Is Current Staff] = '1' AND u.[Is Academic] = '1' AND u.[DeptGroup7] LIKE '%med-%'  
	UNION
	SELECT u.[username] FROM [User] as u WHERE u.[Is Current Staff] = '1' AND u.[Is Academic] = '1' AND u.[PositionType7] LIKE '%med-%' 
	UNION
	SELECT u.[username] FROM [User] as u WHERE u.[Is Current Staff] = '1' AND u.[Is Academic] = '1' AND u.[PositionCat7] LIKE '%med-%' 
	UNION
	SELECT u.[username] FROM [User] as u WHERE u.[Is Current Staff] = '1' AND u.[Is Academic] = '1' AND u.[Dept8] LIKE '%med-%'  
	UNION
	SELECT u.[username] FROM [User] as u WHERE u.[Is Current Staff] = '1' AND u.[Is Academic] = '1' AND u.[DeptClass8] LIKE '%med-%' 
	UNION
	SELECT u.[username] FROM [User] as u WHERE u.[Is Current Staff] = '1' AND u.[Is Academic] = '1' AND u.[DeptGroup8] LIKE '%med-%' 
	UNION
	SELECT u.[username] FROM [User] as u WHERE u.[Is Current Staff] = '1' AND u.[Is Academic] = '1' AND u.[PositionType8] LIKE '%med-%'  
	UNION
	SELECT u.[username] FROM [User] as u WHERE u.[Is Current Staff] = '1' AND u.[Is Academic] = '1' AND u.[PositionCat8] LIKE '%med-%'  
	UNION
	SELECT u.[username] FROM [User] as u WHERE u.[Is Current Staff] = '1' AND u.[Is Academic] = '1' AND u.[Dept9] LIKE '%med-%'  
	UNION
	SELECT u.[username] FROM [User] as u WHERE u.[Is Current Staff] = '1' AND u.[Is Academic] = '1' AND u.[DeptClass9] LIKE '%med-%' 
	UNION
	SELECT u.[username] FROM [User] as u WHERE u.[Is Current Staff] = '1' AND u.[Is Academic] = '1' AND u.[DeptGroup9] LIKE '%med-%'  
	UNION
	SELECT u.[username] FROM [User] as u WHERE u.[Is Current Staff] = '1' AND u.[Is Academic] = '1' AND u.[PositionType9] LIKE '%med-%'  
	UNION
	SELECT u.[username] FROM [User] as u WHERE u.[Is Current Staff] = '1' AND u.[Is Academic] = '1' AND u.[PositionCat9] LIKE '%med-%' 
	) as secondaryMED

WHERE secondaryMED.[username] NOT IN (SELECT primaryMED.[username] FROM (SELECT u.[username] FROM [User] as u WHERE u.[Is Current Staff] = '1' AND u.[Is Academic] = '1' AND u.[Dept1] LIKE '%med-%' 
											UNION 
											SELECT u.[username] FROM [User] as u WHERE u.[Is Current Staff] = '1' AND u.[Is Academic] = '1' AND u.[Dept0] LIKE '%med-%' 
											UNION
											SELECT u.[username] FROM [User] as u WHERE u.[Is Current Staff] = '1' AND u.[Is Academic] = '1' AND u.[DeptClass0] LIKE '%med-%' 
											UNION
											SELECT u.[username] FROM [User] as u WHERE u.[Is Current Staff] = '1' AND u.[Is Academic] = '1' AND u.[DeptGroup0] LIKE '%med-%' 
											UNION
											SELECT u.[username] FROM [User] as u WHERE u.[Is Current Staff] = '1' AND u.[Is Academic] = '1' AND u.[PositionType0] LIKE '%med-%' 
											UNION
											SELECT u.[username] FROM [User] as u WHERE u.[Is Current Staff] = '1' AND u.[Is Academic] = '1' AND u.[PositionCat0] LIKE '%med-%'
											) as primaryMED)