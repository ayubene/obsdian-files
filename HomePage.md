```dataviewjs
let pages = dv.pages("#Done");
dv.table(
	["Name"],
	pages.where(b => b.tags[0]=="Blog")
		.map(b => [b.file.link])
)
```