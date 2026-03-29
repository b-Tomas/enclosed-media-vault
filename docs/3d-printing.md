# 3D printing

## Custom designs

All source files are FreeCAD projects. STLs are included for direct slicing.

| Component    | Description                                                                                                                                                                                                               | Printables                                                                                  | Source                                      |
| ------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------- | ------------------------------------------- |
| Base chassis | 10" 2U open chassis, designed for 220x220mm print beds. U-shaped tray with rack ears and M3 threaded insert holes for standard 10" rails.                                                                                 | [Printables](https://www.printables.com/model/1639121-10-2u-chassis-base-for-220x220mm-bed) | [`CAD/base-chassis/`](../CAD/base-chassis/) |
| Drive box    | Holds 6x 2.5" drives vertically. Consists of a base tray and separating ribs that slot between drives. Ventilation slots on top.                                                                                          | [Printables](https://www.printables.com/model/1639132-6x-25-drive-box)                      | [`CAD/drives/`](../CAD/drives/)             |
| NAS case     | 2U enclosed case with 5 pieces: body, front panel, back panel, cover, and HDMI cutout. Front panel has USB and HDMI passthrough; back panel has an IEC C14 power inlet cutout. Houses the drive box, Pi, and all cabling. | <!-- TODO: upload to Printables -->                                                         | [`CAD/case/`](../CAD/case/)                 |

### STL files

**Base chassis:** `10-inch-2U-chassis-base-Base.stl`

**Drive box:** `drives-base.stl`, `drives-rib.stl`

**NAS case:** `case-Body.stl`, `case-Front.stl`, `case-Back.stl`, `case-Cover.stl`, `case-HDMI.stl`

### Pending CAD work

See the issues page.

## Third-party prints used

- [Mini Rack 10" server rack](https://www.printables.com/model/1210194) by StreelyDan. The rack frame itself, which happened to be compatible with the 6U rails I bought
- [M.2 2230/2242/2260 to 2280 adapter](https://www.printables.com/model/217264-223022422260-to-2280-m2-adapter-ssd) by nzalog

## Other references explored

Designs that were reviewed but not printed. Kept here for future reference.

**Cases:**

- [10" mini rack with solid side walls](https://www.printables.com/model/1261430)

**Ventilation:**

- [10" rack vent panels](https://makerworld.com/en/models/1192269)

**Drive bays (3.5" -- not used, but useful reference):**

- [1U 2x 3.5" HDD mount, no hotswap](https://www.printables.com/model/1320021)
- [1U 2x 3.5" HDD, hotswap](https://www.printables.com/model/1290788)
- [3U 8-drive bay](https://www.thingiverse.com/thing:6930930)
- [2U 4-drive mount with fan support](https://www.printables.com/model/1418516)
