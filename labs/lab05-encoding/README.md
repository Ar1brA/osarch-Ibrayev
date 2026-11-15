# Lab 5 — Encoding forensics
Lab 5 — Encoding forensics

Name: Arman · Date: 15 November 2026 · Tools: Git Bash on Windows (file, xxd, iconv), Excel, LibreOffice Calc

What I received
Excel (double-click on staff-broken.csv): Јбдірахманова, Ќасымов
LibreOffice Calc: the import dialog offered Cyrillic (Windows-1251) by default. Russian names were readable, but the Kazakh letters were wrong ("Јбдірахманова" instead of "Әбдірахманова", "Ќасымов" instead of "Қасымов").
Terminal (cat): <TODO: one line, what you saw in Git Bash> ID;▒▒;▒▒;;▒▒;
101;;;▒▒;▒▒ ;▒▒


I did not save the file in any of them.

Diagnosis (made before the fix)
file said: Non-ISO extended-ASCII text, with CRLF line terminators (evidence/file-before.txt). That means one byte per letter, so a legacy code page; file cannot say which one.
First 16 bytes: 49 44 3b d4 e0 ec e8 eb e8 ff 3b c8 ec ff 3b ce (evidence/hexdump-before.txt)
Encoding: KZ-1048 (iconv name RK1048) — because every Cyrillic letter is a single byte between C0 and FF. In the header "Фамилия" the "Ф" is the single byte D4, "а" is E0, "я" is FF. There are no D0/D1 pairs (so not UTF-8) and no xx 04 pairs (so not UTF-16 LE). It is KZ-1048 and not plain Windows-1251 because of the Kazakh letters: the surname in row 101 starts with the single byte A3, which is "Ә" in KZ-1048 but "Ј" in Windows-1251. I checked both with iconv: decoding as Windows-1251 gives "Јбдірахманова", "Ќасымов", "Жўмабаев"; decoding as RK1048 gives "Әбдірахманова", "Қасымов", "Жұмабаев", and all Russian names are correct too. The bytes where the two code pages differ in this file are 8D 9D A2 A3 A5 AF B4 BA BC BF.
Line ending: CRLF — because the first line ends with the bytes 0d 0a right after "Город" (c3 ee f0 ee e4), and file reports "with CRLF line terminators".
BOM: no — because the file starts with 49 44 ("ID"), not with EF BB BF, FF FE or FE FF.
Separator: ; — the header is ID;Фамилия;Имя;Отдел;Должность;Город.
Decisions for the fixed file
BOM: I added a UTF-8 BOM (EF BB BF). The brief says the file must open correctly everywhere, including Excel by double-click. Excel on Windows only treats a CSV as UTF-8 when it starts with the BOM; without it Excel assumes the local code page and shows Cyrillic and Kazakh letters wrongly. The cost is that a script reading the first column name would see ﻿ID instead of ID.
Line ending: I kept CRLF. The source already used CRLF, the file is meant for Excel and Windows users, and nothing in this lab needs LF. The cost is that a Linux script would see a trailing \r on the last column; sed -i 's/\r$//' removes it if needed.
Fix

scripts/fix.sh, run from labs/lab05-encoding/ with sh scripts/fix.sh:

sh
#!/bin/sh
# Lab 5: convert staff-broken.csv from RK1048 (KZ-1048) to UTF-8
set -e

in=evidence/staff-broken.csv
out=evidence/staff-fixed.csv

# 1. encoding: KZ-1048 (iconv name RK1048) -> UTF-8
iconv -f RK1048 -t UTF-8 "$in" > "$out"

# 2. line endings: source is CRLF, I keep CRLF, so no change here.
#    To get LF instead:  sed -i 's/\r$//' "$out"

# 3. BOM: add the UTF-8 BOM (EF BB BF) so Excel recognises UTF-8 on double-click
printf '\357\273\277' | cat - "$out" > "$out.tmp" && mv "$out.tmp" "$out"

The original evidence/staff-broken.csv was never edited, and the fixed file was produced by this script, not by saving from Excel.

Verification
evidence/file-after.txt: UTF-8 (with BOM) text, with CRLF line terminators. The file starts with ef bb bf 49 44 ("ID"), and Cyrillic letters are D0 xx / D1 xx pairs.
evidence/opens-correctly.png: staff-fixed.csv open in Excel, LibreOffice Calc (Character set UTF-8, separator ;) and in the terminal with cat. Names such as Әбдірахманова, Қасымов, Нұрлан, Жұмабаев and Ерғали are correct in all three. 
What did not work the first time

LibreOffice opened staff-broken.csv with Windows-1251 by default. That looked almost right, because the Russian names were fine, but the Kazakh letters were wrong ("Јбдірахманова", "Ќасымов"). Windows-1251 has no Kazakh letters, so I had to compare the bytes of the Kazakh letters (A3, 8D, A2, ...) with the KZ-1048 table and decode with iconv -f RK1048 instead. The table didn't open in Git Bash right away because I didn't immediately figure out how to do it.
