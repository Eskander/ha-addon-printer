# HPLIP Printer Application

## INTRODUCTION

This repository contains a Printer Application for printing on HP and
Apollo printers supported by HP's HPLIP driver suite.

It uses [PAPPL](https://www.msweet.org/pappl) to support IPP
printing from multiple operating systems. In addition, it uses the
resources of [cups-filters
2.x](https://github.com/OpenPrinting/cups-filters) (filter functions
in libcupsfilters, libppd) and
[pappl-retrofit](https://github.com/OpenPrinting/pappl-retrofit)
(encapsulating classic CUPS drivers in Printer Applications). This
work (or now the code of pappl-retrofit) is derived from the
[hp-printer-app](https://github.com/michaelrsweet/hp-printer-app).

The printer driver itself and the software to communicate with the
printer hardware is taken from the [HPLIP (HP Linux Imaging and
Printing) driver
suite](https://developers.hp.com/hp-linux-imaging-and-printing/), also
the Information about supported printer models and their capabilities.

Your contributions are welcome. Please post [issues and pull
requests](https://github.com/OpenPrinting/hplip-printer-app).

For PostScript printers you can also use the [PostScript Printer
Application](https://github.com/OpenPrinting/ps-printer-app),
especially if you have it already installed for some non-HP PostScript
printer.

Also check whether your printer is a driverless IPP printer (AirPrint,
Mopria, IPP Everywhere, Wi-Fi Direct Print, prints from phones) as in
this case you do not need any Printer Application at all. Most modern
HP printers, even the cheapest models, are driverless IPP
printers. Even USB-only printers can be driverless IPP, and you can
generally use driverless IPP via USB, try
[ipp-usb](https://github.com/OpenPrinting/ipp-usb) for these cases
first.

### Properties

- A Printer Application providing the `hpcups` printer driver and all
  printer's PPDs of HPLIP, supporting printing on most printers from
  HP and Apollo. This allows easy printing in high quality, including
  photos on photo paper. The `hpps` CUPS filter for PIN-protected
  printing on PostScript printers is also included.

- The printers are discovered with HPLIP, too. For USB printers the
  `hp` CUPS backend is used and for network printers the `hp-probe`
  utility (encapsulated in a script to behave as a CUPS backend).

- The communication with the printers is done by the `hp` CUPS backend
  and so (at least in case of USB) the IEEE-1284.4 packet protocol
  (protocol 7/1/3 on USB) is used and not a simple stream protocol
  (like the standard CUPS and PAPPL backends use). This way one should
  be able to print and scan simultaneously, or at least check printer
  status while printing. Not all printers support this protocol, if
  not, a standard streaming protocol is used. Also any other special
  functionality which requires the `hp` backend is supported. On the
  "Add Printer" web interface page under "Devices" select the "HPLIP
  (HP)" entries.

- Note that the `hp` backend does not allow bi-directional access to
  the printer. If you have a PostScript printer and prefer support for
  remote querying of the printer's accessory configuration instead of
  simultaneous printing and scanning, CUPS' standard backends for USB
  and network printers are also available.

- If you have an unusal system configuration or a personal firewall
  HP's backends will perhaps not discover your printer. Also in this
  situation the standard backends, including the fully manual "Network
  Printer" entry in combination with the hostname/IP field can be
  helpful.

- Use of CUPS' instead of PAPPL's standard backends makes quirk
  workarounds for USB printers with compatibility problems being used
  (and are editable) and the output can get sent to the printer via
  IPP, IPPS (encrypted!), and LPD in addition to socket (usually port
  9100). The SNMP backend can get configured (community, address
  scope).

- PWG Raster, Apple Raster or image input data to be printed on a
  non-PostScript printer does not get converted to PostScript or PDF,
  it is only converted/scaled to the required color space and
  resolution and then fed into the `hpcups` driver.

- For printing on non-PostScript printers PDF and PostScript input
  data is rendered into raster data using Ghostscript. Ghostscript is
  also used to convert PDF into PostScript for PostScript printers.

- The information about which printer models are supported and which
  are their capabilities is based on the PPD files included in
  HPLIP. They are packaged in the Snap as a compressed archive.

- Standard job IPP attributes are mapped to the driver's option
  settings best fitting to them so that users can print from any type
  of client (like for example a phone or IoT device) which only
  supports standard IPP attributes and cannot retrive the PPD
  options. Trays, media sizes, media types, and duplex can get mapped
  easily, but when it comes to color and quality it gets more complex,
  as relevant options differ a lot in the PPD files. Here we use an
  algorithm which automatically (who wants hand-edit ~3000 PPD files
  for the assignments) finds the right set of option settings for each
  combination of `print-color-mode` (`color`/`monochrome`),
  `print-quality` (`draft`/`normal`/`high`), and
  `print-content-optimize`
  (`auto`/`photo`/`graphics`/`text`/`text-and-graphics`) in the PPD of
  the current printer. So you have easy access to the full quality or
  speed of your printer without needing to deal with printer-specific
  option settings (the original options are still accessible via web
  admin interface).

- The Snap of the HPLIP Printer Application takes HPLIP's source code
  from Debian's packaging repository instead of directly from HP, as
  Debian's package has ~80 patches fixing bugs which are reported to
  HP but the patch not adopted upstream. So with the Snap users should
  get the same experience in reliability and quality as with the
  Debian package.

- Support for downloading the proprietary plugin of HPLIP via an
  additional page in the web interface. This adds support for some
  laser printers which need their firmware loaded every time they are
  turned on or which use certain proprietary print data formats. This
  works both in the Snap and in the classic installation of the
  Printer Application (must run as root, otherwise only status check
  of the plugin).

<!-- Begin Included Components -->
## Included Components
  - pappl v1.4.8
  - qpdf v11.9.1
  - ghostscript ghostpdl-10.05.0-test-base-001
  - cups v2.4.11
  - libcupsfilters 2.1.0
  - libppd 2.1.0
  - pyppd release-1-1-0
  - hplip debian/3.22.10+dfsg0-5
<!-- End Included Components -->

## LEGAL STUFF

The HPLIP Printer Application is Copyright © 2020 by Till Kamppeter.

It is derived from the HP PCL Printer Application, a first working model of
a raster Printer Application using PAPPL. It is available here:

https://github.com/michaelrsweet/hp-printer-app

The HP PCL Printer Application is Copyright © 2019-2020 by Michael R Sweet.

This software is licensed under the Apache License Version 2.0 with an exception
to allow linking against GPL2/LGPL2 software (like older versions of CUPS).  See
the files "LICENSE" and "NOTICE" for more information.
