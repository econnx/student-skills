---
name: doc-privacy
description: Check a .docx, .xlsx, .pptx or .pdf for hidden data before it is sent or submitted - author name, editing time, comments, tracked changes, hidden or white text, invisible zero-width characters - and optionally make a clean copy. Use when the user asks what a file reveals, whether it has hidden marks or metadata, or wants to clean a file before submitting it.
---

# Doc privacy check

Office files are zip archives of XML. Read them directly; do not open them in an app.

## Check

Run this from the folder with the file (works on macOS and Linux, needs only `unzip` and `python3`):

```bash
python3 - "path/to/file.docx" <<'PY'
import re, sys, zipfile
f = sys.argv[1]
z = zipfile.ZipFile(f)
names = z.namelist()
read = lambda p: z.read(p).decode("utf8", "ignore") if p in names else ""
core, app = read("docProps/core.xml"), read("docProps/app.xml")
tag = lambda x, t: (re.search(rf"<{t}[^>]*>(.*?)</{t}>", x, re.S) or [None, ""])[1]
for t in ["dc:creator", "cp:lastModifiedBy", "cp:revision", "dcterms:created", "dcterms:modified"]:
    print(f"{t}: {tag(core, t)!r}")
for t in ["Company", "Manager", "TotalTime", "Template", "Application"]:
    print(f"{t}: {tag(app, t)!r}")
parts = [p for p in names if re.match(r"word/(document|header\d*|footer\d*|footnotes|endnotes)\.xml$|ppt/(slides|notesSlides)/[^/]+\.xml$|xl/(sharedStrings|worksheets/[^/]+)\.xml$", p)]
xml = "".join(read(p) for p in parts)
text = re.sub(r"<[^>]+>", "", xml)
zw = re.findall("[​‌‍⁠﻿᠎]", text)
print("invisible characters:", len(zw), {hex(ord(c)): zw.count(c) for c in set(zw)})
print("hidden text runs:", len(re.findall(r'<w:vanish/>|<w:vanish w:val="(?:1|true|on)"/>', xml)))
print("white text runs:", len(re.findall(r'<w:color w:val="FFFFFF"', xml, re.I)))
print("tiny text runs:", len(re.findall(r'<w:sz w:val="[1-3]"/>', xml)))
print("tracked insertions/deletions:", xml.count("<w:ins "), xml.count("<w:del "))
print("comments:", read("word/comments.xml").count("<w:comment "),
      "| comment files:", [p for p in names if "comment" in p.lower()])
print("hidden sheets:", re.findall(r'<sheet [^>]*name="([^"]+)"[^>]*state="(?:hidden|veryHidden)"', read("xl/workbook.xml")))
print("speaker notes:", len([p for p in names if p.startswith("ppt/notesSlides/")]))
print("embedded media:", len([p for p in names if "/media/" in p]))
PY
```

For a PDF, read the metadata with `mdls` on macOS or `pdfinfo` if it is installed, or search the raw file:
`strings file.pdf | grep -E "/(Author|Creator|Producer|Title|CreationDate|ModDate)"`.

## Report

Explain the results in plain words, worst first:

1. **Reveals who you are:** author, last modified by, company, a template path with a username.
2. **Hidden content:** comments, tracked changes (deleted text can be read back), hidden text, white or 1-pt text, hidden sheets, speaker notes.
3. **Invisible characters:** zero-width spaces and joiners. Say how many and where (quote 20 characters around each).
4. **Just information:** dates, editing time, app version.

If everything is clean, say so clearly.

## Clean (only when the user asks)

Never change the original. Make a copy named `<name>-clean.<ext>`:
- blank `dc:creator`, `cp:lastModifiedBy`, `cp:lastPrinted` in `docProps/core.xml`, set `cp:revision` to 1
- blank `Company` and `Manager`, set `TotalTime` to 0 in `docProps/app.xml`
- rewrite every other zip entry unchanged

Tell the user to accept or reject tracked changes and delete comments in Word themselves, because removing them by hand from XML can break the file.

A web version of this check that runs in the browser: https://econnx.github.io/docpeek/
