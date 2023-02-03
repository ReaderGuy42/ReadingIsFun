---
obsidianUIMode: preview
cssClasses: cards, cards-cover, cards-2-3, table-max, max
---
### Pages read
``` dataviewjs
	var labels = ["January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "November", "December"];
	var colors = [['#ff6384'],['#36a2eb'],['#ffce56'],['#4bc0c0'],['#9966ff'],['#ff9f40']]
	var datasets = [];

	for(
		let results of dv.pages('"2 Book Log"')
			.where(p => p.DateFinished && p.Length && p.Medium == "Book"  || p.Medium == "eBook" || p.Medium == "GraphicNovel" || p.Medium == p.Medium == "AcademicBook" || p.Medium == "AcademicArticle")
			.sort(p => p.DateFinished, 'asc')
			.groupBy(p => (dv.date(p.DateFinished).toFormat("MMMM yyyy")))
			.sort(ym => ym.rows.DateFinished.first(), 'asc')
			.groupBy(ym => (dv.date(ym.rows.DateFinished.first()).year))
			.sort(y => y.key, 'asc') 
		) {
			let lbl = "Pages read in " + results.key;
			let backCol = colors[datasets.length%colors.length];
			let bordCol = colors[datasets.length%colors.length];
			let bWidth = 1;

			let innerArray = [0,0,0,0,0,0,0,0,0,0,0,0];
			results.rows.forEach(m => {
				let numBooks = dv.array(m.rows).Length.array().reduce((s, r) => s + r, 0);
				//OR .Length.array().reduce((s,r) => s + r, 0);
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


```dataviewjs
let BookCount = (dv.pages('"2 Book Log"').where(p => p.Medium == "Book" || p.Medium == "eBook" || p.Medium == "GraphicNovel").length);

let pagesum = 0;
   for(let i = 0; i < dv.pages('"2 Book Log"').where(p => p.Medium == "Book" || p.Medium == "eBook" || p.Medium == "GraphicNovel" || p.Medium == "AcademicBook" || p.Medium == "AcademicArticle").length; i++) {
     if(dv.pages('"2 Book Log"').where(p => p.Medium == "Book" || p.Medium == "eBook" || p.Medium == "GraphicNovel" || p.Medium == "AcademicBook" || p.Medium == "AcademicArticle")[i].Length) {
       pagesum += dv.pages('"2 Book Log"').where(p => p.Medium == "Book" || p.Medium == "eBook" || p.Medium == "GraphicNovel" || p.Medium == "AcademicBook" || p.Medium == "AcademicArticle")[i].Length;
         }
   }
   

let today = DateTime.now().toFormat("ooo");

dv.paragraph("# Since 2020 I've read " + BookCount + " books. ")

dv.paragraph("# I've read " + pagesum + " pages.")


```

### Gallery
```dataview
TABLE without id 
      ("![](" + Cover + ")") AS "Cover",
      P_BookByAuthor + P_MediumIcon + P_FavoriteIcon AS "Title",
      Q_YearRating + Q_PagesHours + " || " + Q_FLAG AS "Time and Place",      
      R_Dates AS "Dates",
      S_ReadingTime AS "Reading Time"
      
FROM "2 Book Log"

WHERE Medium = "Book" OR Medium = "eBook" OR Medium = "GraphicNovel" 
SORT DateFinished desc


FLATTEN link(file.link, Alias) + " by " + Author AS P_BookByAuthor
FLATTEN choice(contains(tags, "Favorite"), "💛", "") AS P_FavoriteIcon

FLATTEN {
    "Book": " 📖 ",
    "Audiobook": " 🎧 ",
    "Podcast": " 📻 ",
    "eBook": " 📃 ",
    "GraphicNovel": " 💬 ",
    "AcademicBook": " 📚🎓 ",
    "AcademicArticle": " 📰 "
}[Medium] AS P_MediumIcon



FLATTEN DateStarted + " — " + DateFinished AS R_Dates
FLATTEN choice(((DateFinished - DateStarted).days = 1), "1 day",  choice((DateFinished != DateStarted), (DateFinished - DateStarted).days + " days", "0 days" )) AS S_ReadingTime



FLATTEN Year + " || " + " ⭐ " + Rating + " || " AS Q_YearRating

FLATTEN {
    "Book": Length + " p",
    "Audiobook": Length + " h",
    "Podcast": Length + " h",
    "eBook": Length + " p",
    "GraphicNovel": Length + " p",
    "AcademicBook": Length + " p",
    "AcademicArticle": Length + " p"
}[Medium] AS Q_PagesHours


FLATTEN join(map(split(Country, ", "), (c) => {
AD: "🇦🇩",
AE: "🇦🇪",
AF: "🇦🇫",
AG: "🇦🇬",
AI: "🇦🇮",
AL: "🇦🇱",
AM: "🇦🇲",
AO: "🇦🇴",
AQ: "🇦🇶",
AR: "🇦🇷",
AS: "🇦🇸",
AT: "🇦🇹",
AU: "🇦🇺",
AW: "🇦🇼",
AX: "🇦🇽",
AZ: "🇦🇿",
BA: "🇧🇦",
BB: "🇧🇧",
BD: "🇧🇩",
BE: "🇧🇪",
BF: "🇧🇫",
BG: "🇧🇬",
BH: "🇧🇭",
BI: "🇧🇮",
BJ: "🇧🇯",
BL: "🇧🇱",
BM: "🇧🇲",
BN: "🇧🇳",
BO: "🇧🇴",
BQ: "🇧🇶",
BR: "🇧🇷",
BS: "🇧🇸",
BT: "🇧🇹",
BV: "🇧🇻",
BW: "🇧🇼",
BY: "🇧🇾",
BZ: "🇧🇿",
CA: "🇨🇦",
CC: "🇨🇨",
CD: "🇨🇩",
CF: "🇨🇫",
CG: "🇨🇬",
CH: "🇨🇭",
CI: "🇨🇮",
CK: "🇨🇰",
CL: "🇨🇱",
CM: "🇨🇲",
CN: "🇨🇳",
CO: "🇨🇴",
CR: "🇨🇷",
CU: "🇨🇺",
CV: "🇨🇻",
CW: "🇨🇼",
CX: "🇨🇽",
CY: "🇨🇾",
CYMRU: "🏴󠁧󠁢󠁷󠁬󠁳󠁿",
CZ: "🇨🇿",
DE: "🇩🇪",
DJ: "🇩🇯",
DK: "🇩🇰",
DM: "🇩🇲",
DO: "🇩🇴",
DZ: "🇩🇿",
EC: "🇪🇨",
EE: "🇪🇪",
EG: "🇪🇬",
EH: "🇪🇭",
ENG: "🏴󠁧󠁢󠁥󠁮󠁧󠁿",
ER: "🇪🇷",
ES: "🇪🇸",
ET: "🇪🇹",
FI: "🇫🇮",
FJ: "🇫🇯",
FK: "🇫🇰",
FM: "🇫🇲",
FO: "🇫🇴",
FR: "🇫🇷",
GA: "🇬🇦",
GB: "🇬🇧",
GD: "🇬🇩",
GE: "🇬🇪",
GF: "🇬🇫",
GG: "🇬🇬",
GH: "🇬🇭",
GI: "🇬🇮",
GL: "🇬🇱",
GM: "🇬🇲",
GN: "🇬🇳",
GP: "🇬🇵",
GQ: "🇬🇶",
GR: "🇬🇷",
GS: "🇬🇸",
GT: "🇬🇹",
GU: "🇬🇺",
GW: "🇬🇼",
GY: "🇬🇾",
HK: "🇭🇰",
HM: "🇭🇲",
HN: "🇭🇳",
HR: "🇭🇷",
HT: "🇭🇹",
HU: "🇭🇺",
ID: "🇮🇩",
IE: "🇮🇪",
IL: "🇮🇱",
IM: "🇮🇲",
IN: "🇮🇳",
IO: "🇮🇴",
IQ: "🇮🇶",
IR: "🇮🇷",
IS: "🇮🇸",
IT: "🇮🇹",
JE: "🇯🇪",
JM: "🇯🇲",
JO: "🇯🇴",
JP: "🇯🇵",
KE: "🇰🇪",
KG: "🇰🇬",
KH: "🇰🇭",
KI: "🇰🇮",
KM: "🇰🇲",
KN: "🇰🇳",
KP: "🇰🇵",
KR: "🇰🇷",
KW: "🇰🇼",
KY: "🇰🇾",
KZ: "🇰🇿",
LA: "🇱🇦",
LB: "🇱🇧",
LC: "🇱🇨",
LI: "🇱🇮",
LK: "🇱🇰",
LR: "🇱🇷",
LS: "🇱🇸",
LT: "🇱🇹",
LU: "🇱🇺",
LV: "🇱🇻",
LY: "🇱🇾",
MA: "🇲🇦",
MC: "🇲🇨",
MD: "🇲🇩",
ME: "🇲🇪",
MF: "🇲🇫",
MG: "🇲🇬",
MH: "🇲🇭",
MK: "🇲🇰",
ML: "🇲🇱",
MM: "🇲🇲",
MN: "🇲🇳",
MO: "🇲🇴",
MP: "🇲🇵",
MQ: "🇲🇶",
MR: "🇲🇷",
MS: "🇲🇸",
MT: "🇲🇹",
MU: "🇲🇺",
MV: "🇲🇻",
MW: "🇲🇼",
MX: "🇲🇽",
MY: "🇲🇾",
MZ: "🇲🇿",
NA: "🇳🇦",
NC: "🇳🇨",
NE: "🇳🇪",
NF: "🇳🇫",
NG: "🇳🇬",
NI: "🇳🇮",
NL: "🇳🇱",
NO: "🇳🇴",
NP: "🇳🇵",
NR: "🇳🇷",
NU: "🇳🇺",
NZ: "🇳🇿",
OM: "🇴🇲",
PA: "🇵🇦",
PE: "🇵🇪",
PF: "🇵🇫",
PG: "🇵🇬",
PH: "🇵🇭",
PK: "🇵🇰",
PL: "🇵🇱",
PM: "🇵🇲",
PN: "🇵🇳",
PR: "🇵🇷",
PS: "🇵🇸",
PT: "🇵🇹",
PW: "🇵🇼",
PY: "🇵🇾",
QA: "🇶🇦",
RE: "🇷🇪",
RO: "🇷🇴",
RS: "🇷🇸",
RU: "🇷🇺",
RW: "🇷🇼",
SA: "🇸🇦",
SB: "🇸🇧",
SC: "🇸🇨",
SCOT: "🏴󠁧󠁢󠁳󠁣󠁴󠁿",
SD: "🇸🇩",
SE: "🇸🇪",
SG: "🇸🇬",
SH: "🇸🇭",
SI: "🇸🇮",
SJ: "🇸🇯",
SK: "🇸🇰",
SL: "🇸🇱",
SM: "🇸🇲",
SN: "🇸🇳",
SO: "🇸🇴",
SR: "🇸🇷",
SS: "🇸🇸",
ST: "🇸🇹",
SV: "🇸🇻",
SX: "🇸🇽",
SY: "🇸🇾",
SZ: "🇸🇿",
TC: "🇹🇨",
TD: "🇹🇩",
TF: "🇹🇫",
TG: "🇹🇬",
TH: "🇹🇭",
TJ: "🇹🇯",
TK: "🇹🇰",
TL: "🇹🇱",
TM: "🇹🇲",
TN: "🇹🇳",
TO: "🇹🇴",
TR: "🇹🇷",
TT: "🇹🇹",
TV: "🇹🇻",
TW: "🇹🇼",
TZ: "🇹🇿",
UA: "🇺🇦",
UG: "🇺🇬",
UM: "🇺🇲",
US: "🇺🇸",
UY: "🇺🇾",
UZ: "🇺🇿",
VA: "🇻🇦",
VC: "🇻🇨",
VE: "🇻🇪",
VG: "🇻🇬",
VI: "🇻🇮",
VN: "🇻🇳",
VU: "🇻🇺",
WF: "🇼🇫",
WS: "🇼🇸",
YE: "🇾🇪",
YT: "🇾🇹",
ZA: "🇿🇦",
ZM: "🇿🇲",
ZW: "🇿🇼"
}[c])) AS Q_FLAG

```

