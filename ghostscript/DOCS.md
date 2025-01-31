# Ghostscript Printer Application

### Contained Printer Drivers

- **Ghostscript built-in**:
  ```
  ap3250, appledmp, bj10e, bj10vh, bj10v, bj10, bj200, bj8XXYYZ.upp,
  bjc250gs, bjc600, bjc800, bjc880j, cdj500, cdj550, cdnj500, chp2200,
  cljet5, cp50, declj250, deskjet, dj505j, djet500, dnj650c,
  eplaser-jp, eplaser, eps9high, eps9mid, epsonc, epson, escpage,
  fmlbp, fmpr, gdi, hl1250, hl7x0, ibmpro, imagen, iwhi, jetp3852,
  jj100, la50, la70, la75plus, la75, laserjet, lbp310, lbp320, lbp8,
  lex5700, lex7000, lips2p, lips3, lips4v, lips4, lj250, lj4dithp,
  lj4dith, lj5gray, ljet2p, ljet3d, ljet3, ljet4d, ljet4, ljetplus,
  ln03, lp2000, lp2563, lp8000, lq850, lxm3200-tweaked, lxm5700m,
  m8510, md1xMono, md2k, md50Eco, md50Mono, md5k, mj700v2c, ml600,
  necp2xX.upp, necp6, npdl, oce9050, oki182, oki4w, okiibm, paintjet,
  pcl3, picty180, pjetxl, pjxl300, pjxl, pj, ps2write, pr150, pr201,
  pxlcolor, pxldpl, pxlmono, r4081, rpdl, sharp.upp, sipixa6.upp,
  sj48, stcolor, t4693dX, tek4696, xes
  ```

  This gives support for many old and ancient printers, but also for
  PCL lasers (especially PCL 6/XL) and dot-matrix printers.

  The included Ghostscript can have more drivers but these are the
  ones for which we have PPD support from Foomatic and which are not
  considered deprecated by another driver.

- **`hpijs` of HPLIP**: For non-HP PCL printers (use the [HPLIP
  Printer Application](https://snapcraft.io/hplip-printer-app) for HP
  printers). Better print quality than with Ghostscript's built-in
  drivers.

- **`pnm2ppa`**: Driver for some older HP printers with proprietary
  protocol, probably the only HP printers **NOT supported by
  HPLIP**. even not with HP's proprietary plugin. The configuration
  file in the Snap is user-editable, see below.

- **`pxljr`**: For HP Color LaserJet 3500/3550/3600, should give
  better output quality than HPLIP.

- **`foo2zjs`**: Driver for laser printers with proprietary languages,
  from Dell, Epson, Fuji, HP, Kyocera, Lexmark, (Konica) Minolta, Oki,
  Olivetti, Ricoh, Samsung, Xerox. Note that the firmware loading
  facility for HP is not included in this Printer Application, use the
  [HPLIP Printer Application](https://snapcraft.io/hplip-printer-app)
  (download proprietary plugin in-app, via web interface) for these
  printers, the firmware is proprietary anyway. In the Snap the user
  can add color profiles, see below.

- **`SpliX`**: Driver for laser printers with proprietary languages,
  from Dell, Lexmark, Samsung, Toshiba, Xerox

- **`brlaser`**: Driver for Brother HL, DCP, and MFC laser printers
  which do not understand PostScript nor PCL.

- **`fxlinuxprint`**: Driver from Fuji Xerox for their PDF printers
  before the times of driverless IPP.

- **`c2esp`**: Driver for Kodak EasyShare printers.

- **`rastertosag-gdi`**: For the few Ricoh models which do not do
  PostScript or PCL: Ricoh Aficio SP 1000S/1100S

- **`dymo`**: Driver for Dymo's label printers

- **`ptouch`**: Driver for Brother's P-Touch label printers

- **`c2050`, `cjet`, `min12xxw`, `m2300w`**: Drivers for older
  Lexmark, Canon, and Minolta printers. For `m2300w` in the Snap the
  user can add color profiles, see below.

- **`CUPS`, `cups-filters`**: Included drivers for PCL, dot-matrix
  (Oki, Epson), label printers (Dymo, Intellitech, Zebra), and some
  older HP DesignJet large-format printers.

- **Manufacturer-supplied PPD files**: For PCL and PDF printers from
  Ricoh and OEM (Gestetner, InfoPrint, Infotec, Lanier, NRG, Ricoh,
  Savin) and from Samsung.

## Included Components
  - pappl v1.4.8
  - qpdf v11.9.1
  - ghostscript ghostpdl-10.05.0-test-base-001
  - cups v2.4.11
  - libcupsfilters 2.1.0
  - libppd 2.1.0
  - cups-filters 2.0.1
  - pyppd release-1-1-0
  - foomatic-db 20240504
  - hplip debian/3.22.10+dfsg0-5
  - c2050 debian/0.3-7
  - cjet debian/0.8.9-11
  - min12xxw debian/0.0.9-11
  - pnm2ppa debian/1.13-13
  - c2esp debian/27-11
  - dymo-cups-drivers debian/1.4.0-12
  - foo2zjs debian/20200505dfsg0-3
  - fxlinuxprint debian/1.1.0+ds-4
  - m2300w debian/0.51-15
  - printer-driver-oki 1.0.2
  - pxljr debian/1.4+repack0-6
  - rastertosag-gdi debian/0.1-8
  - splix debian/2.0.1-1
  - brlaser v6
  - ptouch-driver debian/1.7-1
<!-- End Included Components -->

## LEGAL STUFF

The Ghostcript Printer Application is Copyright © 2020 by Till Kamppeter.

It is derived from the HP PCL Printer Application, a first working model of
a raster Printer Application using PAPPL. It is available here:

https://github.com/michaelrsweet/hp-printer-app

The HP PCL Printer Application is Copyright © 2019-2020 by Michael R Sweet.

This software is licensed under the Apache License Version 2.0 with an exception
to allow linking against GPL2/LGPL2 software (like older versions of CUPS).  See
the files "LICENSE" and "NOTICE" for more information.
