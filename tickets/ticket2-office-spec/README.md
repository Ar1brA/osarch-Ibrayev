# Ticket 2 — Office hardware specification

## 1. What was asked

> **From:** Daniyar K. (Director)
> **Subject:** computers for the new office
>
> We are opening the second office and need four computers — accountant, sales, web developer, designer. Budget 3,000,000 tenge for everything. Everyone says we should just buy the same four machines, is that right? Make sure they last a few years. Monitors included.

## 2. What was actually needed

**Four identical machines are wrong.** The four people load a computer in four different ways. The accountant needs reliability and a clear screen. Sales needs memory for browser tabs and video calls. The developer needs memory and CPU cores for containers. The designer needs a colour-accurate monitor first. One "average" machine overspends on the accountant and underspends on the designer, and it puts the bottleneck of each person in a different place. Same platform for all four (one board, one PSU, one case) is a different matter: it is good, because it makes support and spare parts simple.

**What the request left out:**
- The operating system. A licensed Windows 11 Pro costs about 72,000 KZT per machine, which is not a small number at this budget.
- Keyboards, mice, a webcam or headset for the sales manager's calls, cables.
- A UPS (power protection), backup storage for the accountant's data, and delivery.
- What "a few years" means, and what software each person needs.

**What I would ask Daniyar:**
1. Which accounting software does the accountant use (does it need Windows)?
2. Does the designer work with photos and layouts, or also with video and 3D?
3. Is "a few years" three years or five? Can the office buy one more part later?
4. Are keyboards, mice and a UPS inside the 3,000,000, or separate?
5. Does the sales manager need a webcam and headset for calls?

**What I assumed instead:**
- "A few years" means about four years, so I chose the AM5 platform and DDR5, which can take a newer CPU and more memory later.
- The accountant uses Windows-only software, so I gave the accountant, sales manager and designer Windows 11 Pro. The developer gets Ubuntu.
- The designer does 2D photo and layout work, not long video editing, so I did not buy a discrete GPU.
- 3,000,000 KZT includes monitors and operating systems; I left a reserve of 355,731 KZT for peripherals.

## 3. What I did

I started from the four roles, not from a parts list. For each role I wrote the load, the bottleneck and the trade-off first ([builds.md](builds.md)), and then chose parts so the money went to the bottleneck. All four machines use the same AM5 platform. I checked compatibility (socket, memory type, M.2, power) with PCPartPicker; PCPartPicker does not show prices in tenge, so all prices come from vendors in Kazakhstan, each with the date I saw it ([budget.csv](budget.csv)). The grand total is **2,644,269 KZT**, so 355,731 KZT is left in the budget.

| Role | Total, KZT | Main money went to |
|---|---|---|
| Accountant | 533 517 | Windows licence, 27" monitor |
| Sales | 646 962 | 32 GB memory |
| Developer | 625 873 | 8-core CPU, 32 GB memory, QHD monitor |
| Designer | 754 062 | Colour monitor, 32 GB memory |

### The director's three questions

**1. Which machine would I upgrade first, eighteen months from now, and why?**
The designer's, and the part is the graphics card. The build has only integrated graphics, and I chose AM5, a mATX board with a PCIe x16 slot and a 650 W PSU precisely so a discrete GPU can be added without replacing anything else. Image and video work only gets heavier, and a graphics card is the single upgrade that would change this person's day.

**2. Pre-built machines from a vendor: what does the company gain and lose?**

| Pre-built machine  | Price, KZT | Seen on |
|---|---|---|
| PULSER Standard 178M (Ryzen 5 5600, 16 GB DDR4, 512 GB SSD, RX 550) | 266,668 | 2026-11-09 |
| Dell Pro Tower QCT1255 (Ryzen 5 8600G, 16 GB DDR5, 512 GB SSD) | 505,511 | 2026-11-09 |
| ITP Game 12 (Ryzen 5 5600X, 32 GB DDR4, 1 TB SSD, RTX 5060) | 509,650 | 2026-11-09 |

None of these includes a monitor, and none states the operating system, and two have only a 512 GB SSD, below the 1 TB requirement. So four pre-built machines would still need monitors and licences on top.
*Gain:* one vendor, one warranty, someone to call, and no assembly time for the company.
*Loss:* choice of parts (I cannot put the money on the bottleneck), an upgrade path (older platforms like AM4/DDR4 in two of the three), and price per useful part. Four machines that fit the roles cost about 2.64 million built to the load; I could not find pre-built machines that meet every requirement at that price.

**3. One of the four people will complain about performance within a month. Who, and about what?**
The designer, about speed with large files and anything that uses the graphics card: effects and filters in big images and exporting video will be slow on integrated graphics. This is the trade-off I accepted to afford the colour monitor. (The second candidate is the accountant: with a single 16 GB module a very large spreadsheet next to the accounting program and a browser could start to feel slow, but that one is cheap to fix with a second module.)

## 4. Evidence

- [budget.csv](budget.csv): every part, price in KZT, vendor link, and the date the price was seen. Proves the total is within 3,000,000 KZT.
- [builds.md](builds.md): the load, bottleneck, trade-off and total for each of the four builds.
- [evidence/pcpartpicker-links.md]: one PCPartPicker list per build. Proves the parts are compatible and the power is enough 
- [evidence/vendor-quotes/](evidence/vendor-quotes/): a screenshot of each price I used, with the vendor's site and the date visible.

## 5. What I would do differently

I would still opt for DDR4 memory; in my view, it will remain relevant for at least another five years, and it’s more cost-effective—which would allow you to put the savings toward a good graphics card for a designer.
