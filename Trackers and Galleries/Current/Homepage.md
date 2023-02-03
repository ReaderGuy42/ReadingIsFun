---
cssClasses: myhome, cards, cards-cover, cards-2-3
banner: "Trackers and Galleries/Things/Attachments/Banners/home-books.jpg"
obsidianUIMode: preview
banner_y: 0.22691
---
# My Books and Stuff
- **Books**

	-  [New Book](obsidian://advanced-uri?vault=Books%20and%20Movies&commandid=obsidian-book-search-plugin%253Aopen-book-search-modal)
	- [[Book Gallery since 2020|Gallery]]
	- [[0 Currently Reading|Current]]
	- [[Reading Stats|Stats]]
	- [[2020]]
	- [[2021]]
	- [[2022]]
	- [[2023]]
- **Movies and Games**
	- [New Movie](obsidian://advanced-uri?vault=Books%20and%20Movies&commandid=quickadd%253Achoice%253Ad33aa448-645b-4990-992e-429df752fef5)
	- [[Movies and Games Gallery|Movies]]
	- [[Movie and Games Stats|Stats]]
	- [[Game Gallery|Games]]


#### Latest Books Finished
```dataview
TABLE without id 
("![](" + Cover + ")") as Cover,
link(file.link, Alias) + " by " + Author + (choice(medium = "Book", " 📖 ", choice(medium = "Audiobook", " 🎧 ", choice(medium = "Podcast", " 📻 ", choice(medium = "eBook", " 📃 ", ""))))) + choice(contains(tags, "Favorite"), "💛", "") as Title, 
Year + " || " + " ⭐ " + Rating + " || " + (choice(medium = "Book", Length + " p", choice(medium = "Audiobook", Length + " h", choice(medium = "Podcast", Length + " h", choice(medium = "eBook", Length + " p", ""))))) + " || " + Country as "Time and Place"
FROM "2 Book Log"
WHERE Medium = "Book" OR Medium = "eBook" OR Medium = "GraphicNovel" OR Medium = "Audiobook" OR Medium = "Podcast" OR Medium = "AcademicBook"  OR Medium = "AcademicArticle"
SORT DateFinished desc
LIMIT 3
```

#### Last Book added
```dataview
TABLE without id 
("![](" + Cover + ")") as Cover,
link(file.link, Alias) + " by " + Author + (choice(medium = "Book", " 📖 ", choice(medium = "Audiobook", " 🎧 ", choice(medium = "Podcast", " 📻 ", choice(medium = "eBook", " 📃 ", ""))))) + choice(contains(tags, "Favorite"), "💛", "") as Title, 
Year + " || " + " ⭐ " + Rating + " || " + (choice(medium = "Book", Length + " p", choice(medium = "Audiobook", Length + " h", choice(medium = "Podcast", Length + " h", choice(medium = "eBook", Length + " p", ""))))) + " || " + Country as "Time and Place"
FROM "1 Currently Reading"
WHERE Medium = "Book" OR Medium = "eBook" OR Medium = "GraphicNovel" OR Medium = "Audiobook" OR Medium = "Podcast" OR Medium = "AcademicBook"  OR Medium = "AcademicArticle"
SORT DateStarted desc
LIMIT 3
```


#### Newest Movies watched
```dataview
TABLE without id 
("![](" + Cover + ")") as Cover,
link(file.link, Alias) + " by " + Director + (choice(medium = "Movie", " 🎞️ ", choice(medium = "TV", " 📺 ", choice(medium = "Game", " 🎮 ", choice(medium = "eBook", " 📃 ", ""))))) + choice(contains(tags, "Favorite"), "💛", "") as Title, 
Year + " || " + " ⭐ " + Rating + " || " + (choice(medium = "Book", Length + " p", choice(medium = "Audiobook", Length + " h", choice(medium = "Podcast", Length + " h", choice(medium = "eBook", Length + " p", ""))))) + " || " + Country as "Time and Place"
FROM "3 Movies and Games"
WHERE Medium = "Movie" OR Medium = "TV" OR Medium = "Game"
SORT DateFinished desc
LIMIT 3
```
