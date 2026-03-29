# Hardware alternatives

The Raspberry Pi 5 exposes a single PCIe 2.0 x1 lane. Connecting both an NVMe boot drive and multiple SATA storage drives from that one lane requires a PCIe switch as a workaround. This document covers the approaches considered and the one ultimately chosen.

## Chosen: Geekworm X1004 dual M.2 HAT

The final build uses the [Geekworm X1004](https://www.amazon.com/Geekworm-X1004-Peripheral-Board-Raspberry/dp/B0D3LP9KBH), a dual M.2 HAT that sits on top of the Pi 5. It has an ASM1182e PCIe switch on-board, splitting the single PCIe lane into two M.2 slots:

- **Slot 1:** 128GB 2242 NVMe SSD OEM drive bought from a shop selling scrap parts (boot drive)
- **Slot 2:** [M.2 to SATA 6-Port adapter](https://www.amazon.com/Adapter-RIITOP-Expansion-Chipset-ASM1166/dp/B0D8BCWHPT) (ASM1166 chipset) -> 6x SATA

Power is supplied by an [5V 15A PSU](https://www.amazon.com/dp/B078RT3ZPS) (I went for the biggest one, but I am not sure I would trust 15A on it), with [SATA splitter cables](https://www.amazon.com/dp/B0DFRJT3CW) distributing power to the drives (soldering required).

The original Raspberry Pi M.2 HAT (single slot, PCIe 2.0) was purchased earlier and used for a year or so when I was using a single drive on a USB adapter, but it not used in the final build.

### PCIe topology

```
$ lspci
0001:00:00.0 PCI bridge: Broadcom Inc. and subsidiaries BCM2712 PCIe Bridge (rev 21)
0001:01:00.0 PCI bridge: ASMedia Technology Inc. ASM1182e 2-Port PCIe x1 Gen2 Packet Switch
0001:02:03.0 PCI bridge: ASMedia Technology Inc. ASM1182e 2-Port PCIe x1 Gen2 Packet Switch
0001:02:07.0 PCI bridge: ASMedia Technology Inc. ASM1182e 2-Port PCIe x1 Gen2 Packet Switch
0001:03:00.0 Non-Volatile memory controller: Shenzhen Unionmemory Information System Ltd. AM620 PCIe 3.0 NVMe SSD 128GB (rev 03)
0001:04:00.0 SATA controller: ASMedia Technology Inc. ASM1166 Serial ATA Controller (rev 02)
0002:00:00.0 PCI bridge: Broadcom Inc. and subsidiaries BCM2712 PCIe Bridge (rev 21)
0002:01:00.0 Ethernet controller: Raspberry Pi Ltd RP1 PCIe 2.0 South Bridge
```

## Alternative A: FPC PCIe switch + dedicated SATA HAT

Use a separate FPC-based PCIe switch HAT to split the lane, then attach both the existing M.2 HAT and a dedicated SATA HAT.

| Component                              | Price     | Link                                                                               |
| -------------------------------------- | --------- | ---------------------------------------------------------------------------------- |
| GeeekPi RPi5 B12 Dual FPC PCIe HAT     | USD 24.99 | [Amazon](https://www.amazon.com/GeeekPi-HAT-Raspberry-B12-Interface/dp/B0D2VTL9G5) |
| Radxa Penta SATA HAT                   | USD 79.99 | [Amazon](https://www.amazon.com/Radxa-Penta-SATA-HAT-Raspberry/dp/B0DX1HQWB2)      |
| Geekworm X1009 PCIe to 5-port SATA HAT | USD 60.00 | [AliExpress](https://es.aliexpress.com/item/1005009068994117.html)                 |
| Waveshare Pi5 Connector Adapter C      | USD 10.00 | [Waveshare](https://www.waveshare.com/pi5-connector-adapter-c.htm)                 |

**Downsides:** The Penta SATA HAT is expensive and hard to source. The X1009 is more affordable and includes 12V DC input (also powers the Pi; do NOT also power via USB-C), but still requires the FPC switch HAT on top.

## Alternative B: Dual M.2 HAT + M.2-to-SATA adapter

This is the approach that was ultimately chosen (see above). For reference, cheaper alternatives were also considered:

| Component                                     | Price     | Link                                                                                     |
| --------------------------------------------- | --------- | ---------------------------------------------------------------------------------------- |
| Geekworm X1004 Dual M.2 HAT (supports boot)   | USD 29.90 | [Amazon](https://www.amazon.com/Geekworm-X1004-Peripheral-Board-Raspberry/dp/B0D3LP9KBH) |
| Cheaper dual M.2 HAT (likely no boot support) | USD 18.59 | [AliExpress](https://es.aliexpress.com/item/1005007485668597.html)                       |
| Another dual M.2 option                       | USD 25.99 | [AliExpress](https://es.aliexpress.com/item/1005006533152444.html)                       |
| RIITOP M.2 to SATA 6-Port (ASM1166)           | USD 39.99 | [Amazon](https://www.amazon.com/Adapter-RIITOP-Expansion-Chipset-ASM1166/dp/B0D8BCWHPT)  |
| Cheaper M.2 to SATA adapter                   | --        | [AliExpress](https://es.aliexpress.com/item/1005010015675089.html)                       |

## Alternative C: USB 3.0 to SATA

The simplest approach. No PCIe splitting needed. Just plug USB-to-SATA adapters into the Pi's USB 3.0 ports.

| Component                    | Price            | Link                                                                                                                                                               |
| ---------------------------- | ---------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| USB 3.0 to 2.5" SATA adapter | ARS 23,200 (x2) | [MercadoLibre](https://www.mercadolibre.com.ar/cable-sata-usb-30-adaptador-conversor-disco-hdd-ssd-25-color-negro/p/MLA48683516?pdp_filters=item_id:MLA1490397025) |

**Trade-off:** Simple and cheap, but bandwidth-limited by USB 3.0 and no hardware RAID support. Two adapters were purchased early on during experimentation. Originally used with a single WD Elements 2TB drive; considered buying more but moved to the PCIe SATA approach instead.

**Notes:**

- I tried connecting a couple of USB drives at once to move files around, and the controller evetually died
- OMV does not support creeating RAID or ZFS filesystems over USB drives. I experimented with a RAID1 setup with two 1TB drives that performed fine, but I had to create it via CLI.

## Alternative D: Compute Module 5

Use a CM5 with a carrier board that has native SATA ports and an NVMe slot, eliminating the need for PCIe switching entirely.

| Component                             | Price   | Link                                                                         |
| ------------------------------------- | ------- | ---------------------------------------------------------------------------- |
| Exaviz Interceptor Carrier Board v2.0 | ~USD 80 | [Exaviz](https://www.exaviz.com/product-page/interceptor-carrier-board-v2-0) |
| Radxa Taco IO Board                   | --      | [Radxa](https://radxa.com/products/io-board/taco/#techspec)                  |

**Blocker:** The CM5 is hard to obtain, and can get expensive.
