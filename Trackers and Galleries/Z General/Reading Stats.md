---
obsidianUIMode: preview
banner: "Trackers and Galleries/Things/Attachments/Banners/library.jpg"
cssClasses: cards, cards-cover, cards-2-3, table-max, max, myhome
banner_x: 0.5
banner_y: 0.4498
---

-
	-  [Home](obsidian://advanced-uri?vault=Books%20and%20Movies&commandid=homepage%253Aopen-homepage)




```dataviewjs
let BookCount = (dv.pages('"2 Book Log"').where(p => p.Medium == "Book" || p.Medium == "eBook" || p.Medium == "GraphicNovel" || p.Medium == "AcademicBook" || p.Medium == "AcademicArticle").length);

let AudioCount = (dv.pages('"2 Book Log"').where(p => p.Medium == "Audiobook" || p.Medium == "Podcast").length);

let AcademicCount = (dv.pages('"2 Book Log/2023"').where(p => p.Medium == p.Medium == "AcademicBook" || p.Medium == "AcademicArticle").length);

let pagesum = 0;
   for(let i = 0; i < dv.pages('"2 Book Log"').where(p => p.Medium == "Book" || p.Medium == "eBook" || p.Medium == "GraphicNovel" || p.Medium == "AcademicBook" || p.Medium == "AcademicArticle").length; i++) {
     if(dv.pages('"2 Book Log"').where(p => p.Medium == "Book" || p.Medium == "eBook" || p.Medium == "GraphicNovel" || p.Medium == "AcademicBook" || p.Medium == "AcademicArticle")[i].Length) {
       pagesum += dv.pages('"2 Book Log"').where(p => p.Medium == "Book" || p.Medium == "eBook" || p.Medium == "GraphicNovel" || p.Medium == "AcademicBook" || p.Medium == "AcademicArticle")[i].Length;
         }
   }

let hoursum = 0;
	for(let i = 0; i < dv.pages('"2 Book Log"').where(p => p.Medium == "Audiobook" || p.Medium == "Podcast").length;i++) {
     if(dv.pages('"2 Book Log"').where(p => p.Medium == "Audiobook" || p.Medium == "Podcast")[i].Length) {
       hoursum += dv.pages('"2 Book Log"').where(p => p.Medium == "Audiobook" || p.Medium == "Podcast")[i].Length;
         }
   }

let Grade = 0;
	for(let i = 0; i < dv.pages('"2 Book Log"').length;i++) {
     if(dv.pages('"2 Book Log"')[i].Rating) {
       Grade += dv.pages('"2 Book Log"')[i].Rating;
         }
   }
let AvgRating = (Grade / (dv.pages('"2 Book Log"').where(p => p.Medium == "Book" || p.Medium == "eBook" || p.Medium == "Audiobook" || p.Medium == "Podcast" || p.Medium == "GraphicNovel" || p.Medium == "AcademicBook" || p.Medium == "AcademicArticle").length))

let year = DateTime.now().year;
let today = DateTime.now().toFormat("dd");
let daysSince2020 = (year - 2020) * 365 + Number(today);

let BooksIn2020 = dv.pages('"2 Book Log"').where(p => p.DateFinished?.year == 2020).length;
let BooksIn2021 = dv.pages('"2 Book Log"').where(p => p.DateFinished?.year == 2021).length;
let BooksIn2022 = dv.pages('"2 Book Log"').where(p => p.DateFinished?.year == 2022).length;

let BooksThisYear = dv.pages('"2 Book Log"').where(p => p.DateFinished?.year == year).length;

dv.paragraph("#### Since 2020, I've read " + BookCount + " books and I've listened to " + AudioCount + " audiobooks (of which " + dv.pages('"2 Book Log/2023"').where(p => p.Medium == "Podcast").length + " were podcasts), so " + (BookCount + AudioCount) + " regular books and audiobooks read. And I've read " + (AcademicCount) + " academic articles, monographs, etc, for a total " + (BookCount + AudioCount + AcademicCount) + " things read in " + daysSince2020 + ".")

dv.paragraph("#### In " + dv.fileLink("2020") + " I read " + BooksIn2020 + " books and audiobooks.")
dv.paragraph("#### In " + dv.fileLink("2021") + " I read " + BooksIn2021 + " books and audiobooks.")
dv.paragraph("#### In " + dv.fileLink("2022") + " I read " + BooksIn2022 + " books and audiobooks.")

dv.paragraph("#### " + dv.fileLink("2023", false, "This year") + " I've read " + BooksThisYear + " books.")

dv.paragraph("#### I've read " + pagesum + " pages since 2020. And I've listened to " + (hoursum.toFixed(1)) + " hours of audiobooks and podcasts.")

dv.paragraph("#### I've read " + (pagesum / 50).toFixed(1) + " days’ worth since 2020, vs. the actual " + daysSince2020 + " days that have passed. So I’m " + ((pagesum / 50) - daysSince2020).toFixed(1) + " days ahead.")

dv.paragraph("#### 50 pages a day would be 18,250 pages for one year, so I have read " + (pagesum / 18250).toFixed(2) + " year’s worth of books.")

dv.paragraph("#### The average rating for the overall " + (BookCount + AudioCount) + " books and audiobooks is " + AvgRating.toFixed(1) + " out of 10.")

dv.paragraph("#### Average book length: " + (pagesum / BookCount).toFixed(1) + " pages")

dv.paragraph("#### Average audiobook length: " + (hoursum / AudioCount).toFixed(1) + " hours")

dv.paragraph("#### Average pages read per day: " + (pagesum / daysSince2020).toFixed(1) + " pages")

dv.paragraph("#### Average hours listened per day: " + (hoursum / daysSince2020).toFixed(2) + " hours — this does not include the fact that most audiobooks are listened to at at least 2x speed")
```


### Overall Books (and Audiobooks) read
``` dataviewjs
	var labels = ["January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "November", "December"];
	var colors = [['#ff6384'],['#36a2eb'],['#ffce56'],['#4bc0c0'],['#9966ff'],['#ff9f40']]
	var datasets = [];

	for(
		let results of dv.pages('"2 Book Log"')
			.where(p => p.DateFinished) //or p.DateFinished && p.Pages
			.sort(p => p.DateFinished, 'asc')
			.groupBy(p => (dv.date(p.DateFinished).toFormat("MMMM yyyy")))
			.sort(ym => ym.rows.DateFinished.first(), 'asc')
			.groupBy(ym => (dv.date(ym.rows.DateFinished.first()).year))
			.sort(y => y.key, 'asc') 
		) {
			let lbl = "Books read in " + results.key;
			let backCol = colors[datasets.length%colors.length];
			let bordCol = colors[datasets.length%colors.length];
			let bWidth = 1;

			let innerArray = [0,0,0,0,0,0,0,0,0,0,0,0];
			results.rows.forEach(m => {
				let numBooks = dv.array(m.rows).length;
				//OR .Pages.array().reduce((s,r) => s + r, 0);
				innerArray[m.rows.DateFinished.first().month-1] = numBooks;
			})

			let da = {label: lbl, data: innerArray, backgroundColor: backCol,
					  borderColor: bordCol, borderWidth: bWidth};
					
			datasets.push(da)
		}

	const chartData = {
		type: 'line',
		data: {labels: labels, datasets: datasets},
		options: {scales: {yAxis: {suggestedMin: 0, ticks: {stepSize: 1}}}
	}}
	window.renderChart(chartData, this.container);
```

### Three longest books

```dataview
TABLE without id 
("![](" + Cover + ")") as Cover,
link(file.link, Alias) + " by " + Author + (choice(medium = "Book", " 📖 ", choice(medium = "Audiobook", " 🎧 ", choice(medium = "Podcast", " 📻 ", choice(medium = "eBook", " 📃 ", ""))))) + choice(contains(tags, "Favorite"), "💛", "") as Title, 
Year + " || " + " ⭐ " + Rating + " || " + (choice(medium = "Book", Length + " p", choice(medium = "Audiobook", Length + " h", choice(medium = "Podcast", Length + " h", choice(medium = "eBook", Length + " p", ""))))) + " || " + Country as "Time and Place"
FROM "2 Book Log"
WHERE Medium = "Book" OR Medium = "eBook" OR Medium = "GraphicNovel"
SORT Length desc
LIMIT 3
```

### Three longest audiobooks
```dataview
TABLE without id 
("![](" + Cover + ")") as Cover,
link(file.link, Alias) + " by " + Author + (choice(medium = "Book", " 📖 ", choice(medium = "Audiobook", " 🎧 ", choice(medium = "Podcast", " 📻 ", choice(medium = "eBook", " 📃 ", ""))))) + choice(contains(tags, "Favorite"), "💛", "") as Title, 
Year + " || " + " ⭐ " + Rating + " || " + (choice(medium = "Book", Length + " p", choice(medium = "Audiobook", Length + " h", choice(medium = "Podcast", Length + " h", choice(medium = "eBook", Length + " p", ""))))) + " || " + Country as "Time and Place"
FROM "2 Book Log"
WHERE Medium = "Audiobook" OR Medium = "Podcast"
SORT Length desc
LIMIT 3
```


### Publication Years of Books read
```chartsview
#-----------------#
#- chart type    -#
#-----------------#
type: Scatter

#-----------------#
#- chart data    -#
#-----------------#
data: |
  dataviewjs:
  return dv.pages('"2 Book Log"')
           .groupBy(p => ([p.year + '', p.DateFinished??p.file.mday]))
           .map(p => ({'Date Finished': p.key[1], 'Published in': p.key[0], Count: p.rows.length, 'Title': p.rows.alias.array()}))
           .sort(p => p['Date Finished'], 'asc')
           .map(p => ({...p, 'Date Finished': p['Date Finished'].toFormat("W, yyyy-MM-dd")}))
           .array();

#-----------------#
#- chart options -#
#-----------------#
options:
  appendPadding: 30
  size: 5
  shape: 'circle'
  pointStyle:
    fillOpacity: 1
  tooltip:
    fields: ['Date Finished', 'Published in', 'Title']
  yAxis:
    nice: true
    tickMethod: |
      function ({values}) {      
        return values.sort((v1, v2) => parseInt(v1) - parseInt(v2));
      }
    line:
      style:
        stroke: '#aaa'
  xAxis:
    grid:
      line:
        style:
          stroke: '#eee'
    line:
      style:
        stroke: '#aaa'
    label: {}
  colorField: Count
  yField: Published in
  xField: Date Finished


```