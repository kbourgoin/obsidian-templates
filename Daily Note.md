---
created: <% tp.file.creation_date() %>
date: <% moment(tp.file.title, 'YYYY-MM-DD dddd').format('YYYY-MM-DD') %>
week: <% moment(tp.file.title, 'YYYY-MM-DD dddd').format('YYYY-WW') %>
---
tags:: [[+Daily Notes]]

# <% moment(tp.file.title,'YYYY-MM-DD').format("dddd, MMMM DD, YYYY") %>

<< [[<% fileDate = moment(tp.file.title, 'YYYY-MM-DD dddd').subtract(1, 'd').format('YYYY-MM-DD dddd') %>|Yesterday]] | [[<% fileDate = moment(tp.file.title, 'YYYY-MM-DD dddd').add(1, 'd').format('YYYY-MM-DD dddd') %>|Tomorrow]] >>

---
# ✅ Tasks

### What I did (not covered in another note)

* 

### Still be be done

* [ ] Process this note


---
# 🤔 Scratchpad Area

* <% tp.file.cursor() %>

---
# 📝 Note Stats

> [!note]+ Notes Linking Here
> ```dataview
> LIST 
> WHERE contains(file.outlinks, [[<% tp.file.title %>]])
> AND file.name != this.file.name
> SORT file.cday DESC

> [!note]+ Notes Created Today
> ```dataview
> List 
> WHERE file.cday = date("<%tp.date.now("YYYY-MM-DD")%>")
> AND file.name != this.file.name
> SORT file.ctime asc
> ```

> [!note]- Notes Last Touched Today
> ```dataview
> LIST
> WHERE file.mday = date("<%tp.date.now("YYYY-MM-DD")%>")
> AND file.name != this.file.name
> SORT file.mtime asc
> ```
