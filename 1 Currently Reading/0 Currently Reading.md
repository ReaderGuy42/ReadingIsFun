---
obsidianUIMode: preview
cssClasses: cards, cards-cover, cards-2-3, table-max, max
---
### Currently Reading

```dataviewjs
let BookCount = (dv.pages('"0 Currently Reading"').where(p => p.Medium == "Book" || p.Medium == "eBook").length);

let AudioCount = (dv.pages('"0 Currently Reading"').where(p => p.Medium == "Audiobook" || p.Medium == "Podcast").length);

let pagesum = 0;
   for(let i = 0; i < dv.pages('"2 Book Log/2023"').where(p => p.Medium == "Book" || p.Medium == "eBook" || p.Medium == "Article" || p.Medium == "Chapter" || p.Medium == "Monograph" || p.Medium == "GraphicNovel").length; i++) {
     if(dv.pages('"2 Book Log/2023"').where(p => p.Medium == "Book" || p.Medium == "eBook" || p.Medium == "Article" || p.Medium == "Chapter" || p.Medium == "Monograph" || p.Medium == "GraphicNovel")[i].Length) {
       pagesum += dv.pages('"2 Book Log/2023"').where(p => p.Medium == "Book" || p.Medium == "eBook" || p.Medium == "Article" || p.Medium == "Chapter" || p.Medium == "Monograph" || p.Medium == "GraphicNovel")[i].Length;
         }
   }

let academicpages = 0;
   for(let i = 0; i < dv.pages('"2 Book Log/2023"').where(p => p.Medium == "Article" || p.Medium == "Chapter" || p.Medium == "Monograph").length; i++) {
     if(dv.pages('"2 Book Log/2023"').where(p => p.Medium == "Article" || p.Medium == "Chapter" || p.Medium == "Monograph")[i].Length) {
       pagesum += dv.pages('"2 Book Log/2023"').where(p => p.Medium == "Article" || p.Medium == "Chapter" || p.Medium == "Monograph")[i].Length;
         }
   }

let hoursum = 0;
	for(let i = 0; i < dv.pages('"2 Book Log/2023"').where(p => p.Medium == "Audiobook" || p.Medium == "Podcast").length;i++) {
     if(dv.pages('"2 Book Log/2023"').where(p => p.Medium == "Audiobook" || p.Medium == "Podcast")[i].Length) {
       hoursum += dv.pages('"2 Book Log/2023"').where(p => p.Medium == "Audiobook" || p.Medium == "Podcast")[i].Length;
         }
   }

let today = DateTime.now().toFormat("ooo");

dv.paragraph("#### Right now, I'm reading " + BookCount + " books and I'm listening to " + AudioCount + " audiobooks of which " + dv.pages('"2 Book Log/2023/0 Currently Reading"').where(p => p.Medium == "Podcast").length + " are podcasts.")

dv.paragraph("#### I've read " + pagesum + " pages this year. On this day, I should have read " + (today * 50) + " pages, so I've read " + (pagesum - (today * 50)) + " pages more than the goal of 50 pages per day.")

dv.paragraph("#### 50 pages a day would be 18,250 pages for one year, so I have " + (18250 - pagesum) + " pages left to read this year.")

```

```dataview
TABLE without id 
      ("![](" + Cover + ")") AS "Cover",
      P_BookByAuthor + P_MediumIcon + P_FavoriteIcon AS "Title",
      Q_YearRating + Q_PagesHours + " || " + Q_FLAG AS "Time and Place",      
      R_Dates AS "Dates",
      S_ReadingTime AS "Reading Time"
      
FROM "1 Currently Reading"

WHERE Medium != null 
SORT DateStarted desc


FLATTEN link(file.link, Alias) + " by " + Author AS P_BookByAuthor
FLATTEN choice(contains(tags, "Favorite"), "💛", "") AS P_FavoriteIcon

FLATTEN {
    "Book": " 📖 ",
    "Audiobook": " 🎧 ",
    "Podcast": " 📻 ",
    "eBook": " 📃 ",
    "Monograph": " 📚🎓 ",
    "Article": " 📰🎓 ",
    "Chapter": " 📔🎓 ",
    "GraphicNovel": " 💬 "
}[Medium] AS P_MediumIcon



FLATTEN DateStarted + " — " + DateFinished AS R_Dates
FLATTEN "(" + choice((DateFinished != DateStarted), (DateFinished - DateStarted).days + " days", "0 days" ) + ")" AS S_ReadingTime



FLATTEN Year + " || " + " ⭐ " + Rating + " || " AS Q_YearRating

FLATTEN {
    "Book": Length + " p",
    "Audiobook": Length + " h",
    "Podcast": Length + " h",
    "eBook": Length + " p"
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