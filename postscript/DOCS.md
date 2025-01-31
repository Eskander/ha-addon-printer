# PostScript Printer Application

## INTRODUCTION

This repository contains a Printer Application for PostScript printers
that uses [PAPPL](https://www.msweet.org/pappl) to support IPP
printing from multiple operating systems. In addition, it uses the
resources of [cups-filters
2.x](https://github.com/OpenPrinting/cups-filters) (filter functions
in libcupsfilters, libppd) and
[pappl-retrofit](https://github.com/OpenPrinting/pappl-retrofit)
(encapsulating classic CUPS drivers in Printer Applications). This
work (or now the code of pappl-retrofit) is derived from the
[hp-printer-app](https://github.com/michaelrsweet/hp-printer-app).

Your contributions are welcome. Please post [issues and pull
requests](https://github.com/OpenPrinting/ps-printer-app).

### This Printer Application is a working model for

- A non-raster Printer Application: Destination format is PostScript,
  a high-level/vector format. Input data in PostScript or PDF is
  accepted and needed conversion is done without any inbetween raster
  steps.

- A Printer Application which uses the new filter functions of
  cups-filters 2.x. Filter functions are library functions derived
  from the CUPS filters and contain decades of development and
  refinement starting from the introduction of CUPS in 2000.

- A retro-fit Printer Application for classic CUPS drivers, in this
  case the simplest form of only PPD files for PostScript printers. It
  lists PPD files from repositories included in the Snap, loads the
  PPD needed for the actual printer, extracts options from the PPD to
  display them in the web interface, accepts job settings as IPP
  attributes, and inserts the PostScript code provided by the PPD
  correctly into the output data stream.

- A Printer Application which does not pass through raw (input format
  is printer's native format) jobs. To assure that always the
  PostScript code of the PPD file is inserted into the output stream,
  we call the printer's native format
  "application/vnd.printer-specific" which does not exist as input
  format, so "application/postscript" input is forced through the
  pstops() filter function.

- An expandable Printer Application: The user can add PPD files via the
  administration web interface to support additional printer models.

Further properties are:

- To avoid the need to re-invent the code for forking into sub-processes
  so that we can pass data through a sequence of filters, we create a
  filter function to send the data off to the printer and form a chain
  of the actually converting filter function (one of pstops() and
  pdftops()) with this filter function using the filterChain() filter
  function.

- For PWG/Apple Raster input we use raster callbacks so that the
  processing is streaming, allowing for large and even infinitely long
  jobs. We use libppd functions to insert the PPD option's PostScript
  code in the output stream and the filterPOpen() function to create a
  file descriptor for the libppd functions to send data off to the
  device.

- The PostScript Printer Application Snap has all PostScript PPD files
  of the [foomatic-db](https://github.com/OpenPrinting/foomatic-db)
  and [HPLIP](https://developers.hp.com/hp-linux-imaging-and-printing)
  projects built-in, so most PostScript printer PPDs which usually
  come with Linux Distributions. To avoid that this vast number of
  PPDs blows up the size of the Snap, we highly compress them using
  [pyppd](https://github.com/OpenPrinting/pyppd). Note that some PPDs
  use certain CUPS filters for extra functionality. These filters are
  included in the Snap, so the extra functionality is supported (in
  most cases PIN-protected printing). The user can add additional PPDs
  without needing to rebuild the Snap (see below).

- We use the printer's IEEE-1284 device ID to identify at first that
  it is a PostScript printer (via CMD: field) to see whether it is
  supported at all and only then check via make and model whether we
  explicitly support it with a PPD. PostScript printers for which
  there is no PPD get a generic PPD assigned. By the check of the CMD:
  field before make/model lookup we assure that if PostScript is
  provided by an add-on module that the module is actually installed.

- Standard job IPP attributes are mapped to the PPD option settings
  best fitting to them so that users can print from any type of client
  (like for example a phone or IoT device) which only supports
  standard IPP attributes and cannot retrive the PPD options. Trays,
  media sizes, media types, and duplex can get mapped easily, but when
  it comes to color and quality it gets more complex, as relevant
  options differ a lot in the PPD files. Here we use an algorithm
  which automatically (who wants hand-edit ~10000 PPDs for the
  assignments) finds the right set of option settings for each
  combination of `print-color-mode` (`color`/`monochrome`),
  `print-quality` (`draft`/`normal`/`high`), and
  `print-content-optimize`
  (`auto`/`photo`/`graphics`/`text`/`text-and-graphics`) in the PPD of
  the current printer. So you have easy access to the full quality or
  speed of your printer without needing to deal with printer-specific
  option settings (the original options are still accessible via web
  admin interface).

- The printer capabilities for a given printer model (a "driver" in
  the Printer Application) are not static throughout the life of the
  print queue set up in the Printer Application. The user can
  configure via a page in the web admin interface which hardware
  accessories (extra paper trays, duplex unit, finishers, ...) are
  installed on the printer and the Printer Application updates the
  driver data structure and with this the printer capabilities. The
  response to a get-printer-attributes IPP request gets updated
  appropriately.

- PostScript is a full-fledged programming language and many PostScript
  printers allow querying settings of options and the presence of
  installable hardware accessories executing appropriate PostScript
  code. If a setting can get queried, the manufacturer puts the needed
  PostScript code into the PPD file, together with the queriable option.
  These queries are supported by the web interface of the Printer
  Application.

- Available printer devices are discovered (and used) with CUPS'
  backends and not with PAPPL's own backends. This way quirk
  workarounds for USB printers with compatibility problems are used
  (and are editable) and PostScript output can get sent to the printer
  via IPP, IPPS (encrypted!), and LPD in addition to socket (usually
  port 9100). The SNMP backend can get configured (community, address
  scope).

- If you have an unusual system configuration or a personal firewall
  your printer will perhaps not get discovered. In this situation the
  fully manual "Network Printer" entry in combination with the
  hostname/IP field can be helpful.

<!-- Begin Included Components -->
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
<!-- End Included Components -->

## LEGAL STUFF

The PostScript Printer Application is Copyright © 2020 by Till Kamppeter.

It is derived from the HP PCL Printer Application, a first working model of
a raster Printer Application using PAPPL. It is available here:

https://github.com/michaelrsweet/hp-printer-app

The HP PCL Printer Application is Copyright © 2019-2020 by Michael R Sweet.

This software is licensed under the Apache License Version 2.0 with an exception
to allow linking against GPL2/LGPL2 software (like older versions of CUPS).  See
the files "LICENSE" and "NOTICE" for more information.
