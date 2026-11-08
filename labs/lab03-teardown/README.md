# Lab 3 — Teardown, and the same machine from the command line

Part 1 — Ten parts
1. AMD Ryzen 5 7600

What it is: 6 cores / 12 threads, 3.8 GHz base, up to 5.1 GHz boost, 32 MB L3 cache, 65 W TDP. Connects to: socket AM5; its memory controller supports DDR5 only (DDR5-5200 with two DIMMs). Limit: works only in AM5 boards with DDR5 — it cannot use DDR4 and cannot be put in an LGA1700 board. Ships with the AMD Wraith Stealth cooler. Source: https://www.amd.com/ja/products/processors/desktops/ryzen/7000-series/amd-ryzen-5-7600.html
2. Intel Core i5-13400

What it is (verify): 10 cores (6 P-cores + 4 E-cores) / 16 threads, up to 4.6 GHz, 20 MB cache, 65 W base power. Connects to: socket LGA1700; its memory controller supports both DDR4 and DDR5 (the board decides which). Limit: only LGA1700 boards (not AM5); the memory type is fixed by the board, not the CPU. Source:  https://www.intel.com/content/www/us/en/products/sku/230495/intel-core-i513400-processor-20m-cache-up-to-4-60-ghz/specifications.html
3. ASUS PRIME B650M-A

What it is: Micro-ATX board, AMD B650 chipset. Connects to: socket AM5; 4 × DDR5 DIMM slots (dual channel, up to 5600 MT/s with OC, AMD EXPO); 2 × M.2 (Key M, 2242/2260/2280): M.2_1 is PCIe 5.0 x4 with Ryzen 7000/9000, M.2_2 is PCIe 4.0 x4 via the chipset; 4 × SATA 6 Gb/s. Limit: accepts Ryzen 7000, 8000 and 9000 desktop CPUs on AM5 (up to 170 W) and DDR5 only; older BIOS may need a BIOS FlashBack update for newer CPUs. Source: https://asus.com/motherboards-components/motherboards/prime/prime-b650m-a/techspec
4. MSI PRO B760M-A WIFI DDR4

What it is (verify): Micro-ATX board, Intel B760 chipset. Connects to: socket LGA1700; DDR4 DIMM slots ; M.2 and SATA ports — 2 . Limit: 12th/13th/14th-gen Intel CPUs only (check the CPU Support list ; DDR4 modules only, DDR5 will not fit. Source: https://ru.msi.com/Motherboard/PRO-B760M-A-WIFI-DDR4
5. Kingston FURY Beast DDR5-5600, 16 GB

What it is (verify): DDR5 DIMM (288-pin), 16 GB, 5600 MT/s. Voltage: 1.25, 1.35, 1,4 V: Operating temperature: 0-85 C Connects to: desktop DIMM slots of a DDR5 board, one module per channel slot. Limit: only DDR5 boards — the notch is in a different place than on DDR4, so it physically does not fit a DDR4 slot. Actual speed also depends on the CPU's memory controller. Source: https://www.kingston.com/en/memory/gaming/kingston-fury-beast-ddr5-memory?speed=5600mt%2Fs&total%20(kit)%20capacity=16gb&kit=single%20module&cas%20latency=40&dram%20density=16gbit&profile%20type=intel%20xmp&color=black
6. Crucial DDR4-3200, 16 GB DIMM

What it is (verify): DDR4 UDIMM (288-pin), 16 GB, 3200 MT/s, 1.2 V. Connects to: desktop DIMM slots of a DDR4 board. Limit: only DDR4 boards; it cannot be used with the Ryzen 5 7600 / AM5 at all (AM5 has no DDR4 support). Source: https://www.crucial.com/memory/ddr4/ct16g4dfra32a?srsltid=AYCX9vJNCciK2IA5ecF3g3iWMa1T9sPCf775VGMK2wQIi8cXMY_-M4cT
7. Samsung 990 PRO 1 TB

What it is: NVMe SSD, M.2 2280, PCIe 4.0 x4, NVMe 2.0, up to 7,450 MB/s read and 6,900 MB/s write. Connects to: an M.2 Key M slot directly on the board — no SATA cable or power cable. Limit: needs an M.2 NVMe slot; PCIe 4.0 speed is only reached if the slot provides PCIe 4.0 x4 (the PCIe 5.0 slot on the B650M-A is backward compatible). Source: https://download.semiconductor.samsung.com/resources/data-sheet/Samsung_NVMe_SSD_990_PRO_Datasheet_Rev.1.0.pdf
8. Samsung 870 EVO 1 TB

What it is (verify): SATA III SSD, 2.5" form factor, 1 TB, up to 560 MB/s read / 530 MB/s write. Connects to: a SATA port on the board + a SATA data cable + a SATA power cable from the PSU. Limit: the SATA bus caps speed at about 550 MB/s — roughly 13× slower than the 990 PRO's peak read. Source: https://news.samsung.com/kz_ru/samsung_870_evo
9. Corsair RM750e (750 W)

What it is (verify): 750 W ATX power supply, 80 PLUS Gold, fully modular, ATX 3.1 / PCIe 5.1. Connects to: the board (24-pin ATX + CPU 8-pin), drives (SATA power), graphics card (PCIe power). Limit: total output of 750 W — enough for either build below; only one unit exists, so only one build gets it. Source: https://www.corsair.com/eu/en/p/psu/cp-9020295-eu/rme-series-rm750e-fully-modular-low-noise-atx-power-supply-eu-cp-9020295-eu
10. Stock cooler — AMD Wraith Stealth


What it is: the cooler included in the box of the Ryzen 5 7600 (AMD's product page lists "Wraith Stealth" as the included cooler). Connects to: the processor's AM5 socket (mounts on the board's AM5 retention bracket). Limit: sized for the 65 W Ryzen 5 7600 — the amount of heat it can remove is what limits it. Source: https://www.amd.com/ja/products/processors/desktops/ryzen/7000-series/amd-ryzen-5-7600.html


## Result

## What did not work the first time

At least one thing. What I saw, what it turned out to be, what I did about it.

## Evidence

- [evidence/](evidence/) — one line per file on what it proves
