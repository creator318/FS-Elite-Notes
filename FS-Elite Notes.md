
```dataview
list WITHOUT ID "[["+file.name+"|"+file.name+" - "+date+"]]" where regexmatch("Day\d+", file.name) SORT id ASC
```