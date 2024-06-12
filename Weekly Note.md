<%*
const thisWeek = moment(tp.file.title,'YYYY-ww');
const lastWeek = moment(thisWeek).subtract(1, 'W');
const nextWeek = moment(thisWeek).add(1, 'W');
const thisPath = `Daily & Weekly Notes/Weekly Notes/${thisWeek.year()}/`
if (!tp.file.path(true).startsWith(thisPath)) {
	await tp.file.move(thisPath + tp.file.title)
}

// TODO: Instructions for filling out and how to use for missing weeks
-%>
---
created: <% tp.file.creation_date() %>
week: <% thisWeek.format('YYYY-ww MMMM') %>
---
<< [[<% lastWeek.format('YYYY-ww MMMM') %>|Last Week]] | [[<% nextWeek.format('YYYY-ww MMMM') %>|Next Week]] >>
# <% thisWeek.format('[Week ]w 🗓️ MMMM YYYY') %>

###  🟩 Kickoff

### 🏁 Wrapup

---
# 🤝 Meetings

> [!note]+ Meetings With Open Tasks
> ```dataview
> TASK 
> FROM "Meetings"
> WHERE
> 	!completed
> 	AND any(file.tasks, (t) => !t.fullyCompleted)
> 	AND file.name != this.file.name
> GROUP BY file.link
> SORT file.name DESC
> ```

> [!note]+ This Week's Meetings
> ```dataviewjs
> const thisWeek = "<% thisWeek.format('YYYY-ww') %>";
> const toWeekStr = (input) => {
> 	if (input instanceof DateTime) {
> 		return input.toFormat('yyyy-WW');
> 	} else if (typeof input === 'string') {
> 		return dv.date(input.substring(0,10)).toFormat('yyyy-WW');
> 	}
> 	return null;
> }
> 
> let output = [];
> const pages = dv.pages('"Meetings"')
> 	for (const page of pages) {
> 		const pageWeek = toWeekStr(page.date);
> 		if ( pageWeek === thisWeek) {
> 			output = [...output, page.file.link];
> 		}
> };
> dv.list(output);
> ```

---
# 🚧  Projects

```dataviewjs

const thisWeek = "<% thisWeek.format('YYYY-ww') %>";

// Given a file, get the goal/result/status for the given YYYY-ww week
const getWeekEntry = (fileLines, week) => {
	const lines = fileLines.filter(line => line.trim().startsWith(`| ${week} |`));
	if (lines.length == 0) {
		return null; // no entry for this week
	}
	const cols = lines[0]
		.split('|')
		.map(part => part.trim());
	if (cols.length == 0) {
		return null; // improperly formatted line
	}
	return [cols[2], cols[3], cols[4]];
}

let active = [];
let inactive = [];
let pages = dv.pages("#project")
for (const page of pages) {
	let pageName = `[[${page.file.name}]]`;
	const lines = (
			await dv.io.load(page.file.path)
		).split('\n');
	const entry = getWeekEntry(lines, thisWeek);
	if (entry === null) {
		if (page.tags.includes("project/inprogress")) {
			inactive = [...inactive, [pageName, page.mday, page.status]];
		}
	} else {
	const [thisGoal,
		   thisResult,
		   thisStatus] = entry;
	active = [...active, [pageName, ...entry]];
	}
};

dv.header(2, "Active Projects");
dv.table(["Project", "This Week's Goal", "This Week's Result", "Status"], active)

dv.header(4, "Inactive, Ongoing Projects");
dv.table(["Project", "Updated", "Status"], inactive)
```

> [!note]- Projects Ready to be Started
> ```dataview
> TABLE file.mday as Updated, status AS "Status"
> FROM #project/ready
> SORT created DESC
> ```

---
# 🤔 Scratchpad


---
# 📝 Note Stats

> [!note]+ 📬 Note Inbox
> ```dataview
> LIST 
> FROM [[]] AND !outgoing([[]])
> SORT created ASC

> [!note]+ Notes Created This Week
> ```dataview
> List 
> WHERE
> 	file.name != this.file.name
> 	AND ((
> 			created >= date("<% tp.date.weekday("YYYY-MM-DD", 0, tp.file.title, "YYYY-W") %>") AND created <= date("<% tp.date.weekday("YYYY-MM-DD", 6, tp.file.title, "YYYY-W") %>")
> 		) OR (
> 			file.cday >= date("<% tp.date.weekday("YYYY-MM-DD", 0, tp.file.title, "YYYY-W") %>") AND file.cday <= date("<% tp.date.weekday("YYYY-MM-DD", 6, tp.file.title, "YYYY-W") %>")
> 	))
> 	
> SORT file.mtime DESC
> ```

> [!note]- Notes Last Touched This Week
> ```dataview
> LIST
> WHERE file.mtime >= date("<% tp.date.weekday("YYYY-MM-DD", 0, tp.file.title, "YYYY-W") %>")
> AND file.mtime <= date("<% tp.date.weekday("YYYY-MM-DD", 6, tp.file.title, "YYYY-W") %>")
> AND file.name != this.file.name
> SORT file.mtime asc
> ```
