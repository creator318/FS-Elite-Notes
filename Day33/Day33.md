---
date: 4-Apr-25
---

```dataview
LIST WITHOUT ID "[["+file.folder+"/"+file.name+"]]" WHERE file.folder = this.file.folder and file.name != this.file.name SORT file.name ASC
```
