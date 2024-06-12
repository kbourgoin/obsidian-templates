---
created: <% tp.file.creation_date() %>
company: 
location: 
title: 
email: 
website: 
aliases: 
moc:
  - "[[People MOC]]"
---
# <% tp.file.title %>
<% await tp.file.move("/Extras/People/" + tp.file.title) %>

## Notes
- 

## Notes
```dataview
TABLE dateformat(file.cday, "yyyy-MM-dd") as Date, summary AS "Summary"
FROM -"Meetings" AND [[]]
SORT date DESC
```

## Meetings
```dataview
TABLE dateformat(date, "yyyy-MM-dd") as Date, summary AS "Summary"
FROM "Meetings" AND [[]]
SORT date DESC
```