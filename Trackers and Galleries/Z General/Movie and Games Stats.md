---
obsidianUIMode: preview
cssClasses: cards, cards-cover, cards-2-3, table-max, max
---
```dataviewjs
let MovieCount = (dv.pages('"3 Movies and Games"').where(p => p.Medium == "Movie").length);

let TVCount = (dv.pages('"3 Movies and Games"').where(p => p.Medium == "TV").length);

let GameCount = (dv.pages('"3 Movies and Games"').where(p => p.Medium == "Game").length);


let durationsum = 0;
	for(let i = 0; i < dv.pages('"3 Movies and Games"').length;i++) {
     if(dv.pages('"3 Movies and Games"')[i].Length) {
       durationsum += dv.pages('"3 Movies and Games"')[i].Length;
         }
   }

let Grade = 0;
	for(let i = 0; i < dv.pages('"3 Movies and Games"').length;i++) {
     if(dv.pages('"3 Movies and Games"')[i].Rating) {
       Grade += dv.pages('"3 Movies and Games"')[i].Rating;
         }
   }
   
let AvgRating = (Grade / (MovieCount + TVCount));

let today = DateTime.now().toFormat("ooo");

dv.paragraph("#### Since 2022, I've watched " + MovieCount + " movies and I've watched to " + TVCount + " TV shows, for a total of " + durationsum.toFixed() + " minutes, or " + (durationsum / 60).toFixed(2) + " hours.")

dv.paragraph("#### And since 2023 I've completed " + GameCount + " Games.")


dv.paragraph("#### Average Rating: " + AvgRating.toFixed(1))
```
### Movies and TV Shows Watched

```dataviewjs
	var labels = ["January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "November", "December"];
	var colors = [['#ff6384'],['#36a2eb'],['#ffce56'],['#4bc0c0'],['#9966ff'],['#ff9f40'], ['#d39e9e'], ['#020000'], ['#f02f95'], [' 	#536c40'], ['#996236']]
	var datasets = [];

	for(
		let results of dv.pages('"3 Movies and Games"')
			.where(p => p.DateFinished && p.Medium == "TV") 
			.sort(p => p.DateFinished, 'asc')
			.groupBy(p => (dv.date(p.DateFinished).toFormat("MMMM yyyy")))
			.sort(ym => ym.rows.DateFinished.first(), 'asc')
			.groupBy(ym => (dv.date(ym.rows.DateFinished.first()).year))
			.sort(y => y.key, 'asc') 
		) {
			let lbl = "TV Shows watched in " + results.key;
			let backCol = colors[datasets.length%colors.length];
			let bordCol = colors[datasets.length%colors.length];
			let bWidth = 1;

			let innerArray = [0,0,0,0,0,0,0,0,0,0,0,0];
			results.rows.forEach(m => {
				let numTV = dv.array(m.rows).length;
				//OR .Pages.array().reduce((s,r) => s + r, 0);
				innerArray[m.rows.DateFinished.first().month-1] = numTV;
			})

			let da = {label: lbl, data: innerArray, backgroundColor: backCol,
					  borderColor: bordCol, borderWidth: bWidth, borderDash: [5,5]};
					
			datasets.push(da)
		}

for(
		let results of dv.pages('"3 Movies and Games"')
			.where(p => p.DateFinished && p.Medium == "Movie") 
			.sort(p => p.DateFinished, 'asc')
			.groupBy(p => (dv.date(p.DateFinished).toFormat("MMMM yyyy")))
			.sort(ym => ym.rows.DateFinished.first(), 'asc')
			.groupBy(ym => (dv.date(ym.rows.DateFinished.first()).year))
			.sort(y => y.key, 'asc') 
		) {
			let lbl = "Movies watched in " + results.key;
			let backCol = colors[datasets.length%colors.length];
			let bordCol = colors[datasets.length%colors.length];
			let bWidth = 1;

			let innerArray = [0,0,0,0,0,0,0,0,0,0,0,0];
			results.rows.forEach(m => {
				let numMovie = dv.array(m.rows).length;
				//OR .Pages.array().reduce((s,r) => s + r, 0);
				innerArray[m.rows.DateFinished.first().month-1] = numMovie;
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

### Hours watched
``` dataviewjs
	var labels = ["January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "November", "December"];
	var colors = [['#ff6384'],['#36a2eb'],['#ffce56'],['#4bc0c0'],['#9966ff'],['#ff9f40']]
	var datasets = [];

	for(
		let results of dv.pages('"3 Movies and Games"')
			.where(p => p.DateFinished && p.Length && p.Medium == "TV")
			.sort(p => p.DateFinished, 'asc')
			.groupBy(p => (dv.date(p.DateFinished).toFormat("MMMM yyyy")))
			.sort(ym => ym.rows.DateFinished.first(), 'asc')
			.groupBy(ym => (dv.date(ym.rows.DateFinished.first()).year))
			.sort(y => y.key, 'asc') 
		) {
			let lbl = "TV Hours in " + results.key;
			let backCol = colors[datasets.length%colors.length];
			let bordCol = colors[datasets.length%colors.length];
			let bWidth = 1;

			let innerArray = [0,0,0,0,0,0,0,0,0,0,0,0];
			results.rows.forEach(m => {
				let numMovies = dv.array(m.rows).Length.array().reduce((s, r) => s + r, 0) / 60;
				
				innerArray[m.rows.DateFinished.first().month-1] = numMovies;
			})

			let da = {label: lbl, data: innerArray, backgroundColor: backCol,
					  borderColor: bordCol, borderWidth: bWidth, borderDash: [5,5]};
					
			datasets.push(da)
		}

	for(
		let results of dv.pages('"3 Movies and Games"')
			.where(p => p.DateFinished && p.Length && p.Medium == "Movie")
			.sort(p => p.DateFinished, 'asc')
			.groupBy(p => (dv.date(p.DateFinished).toFormat("MMMM yyyy")))
			.sort(ym => ym.rows.DateFinished.first(), 'asc')
			.groupBy(ym => (dv.date(ym.rows.DateFinished.first()).year))
			.sort(y => y.key, 'asc') 
		) {
			let lbl = "Movie Hours in " + results.key;
			let backCol = colors[datasets.length%colors.length];
			let bordCol = colors[datasets.length%colors.length];
			let bWidth = 1;

			let innerArray = [0,0,0,0,0,0,0,0,0,0,0,0];
			results.rows.forEach(m => {
				let numMovies = dv.array(m.rows).Length.array().reduce((s, r) => s + r, 0) / 60;
				
				innerArray[m.rows.DateFinished.first().month-1] = numMovies;
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
  return dv.pages('"3 Movies and Games"')
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