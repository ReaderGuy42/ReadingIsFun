---
obsidianUIMode: preview
cssClasses: cards, cards-cover, cards-2-3, table-max, max
---


# Want to Read
```dataview
TABLE without id 
      ("![](" + Cover + ")") AS "Cover",
      P_BookByAuthor AS "Title"
      
FROM "Trackers and Galleries/Want to Read"
WHERE Medium != null


SORT Author desc


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





```

