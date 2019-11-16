# CUPS安装文件

```
/etc/cups
|-- cups-browsed.conf
|-- cups-files.conf
|-- cups-files.conf.default
|-- cupsd.conf
|-- cupsd.conf.default
|-- cupsd.conf_allow_all
|-- ppd
|-- snmp.conf
|-- snmp.conf.default
`-- ssl
```

```
/usr/lib/cups
|-- backend
|   |-- beh
|   |-- cups-brf
|   |-- dnssd
|   |-- driverless -> ../../usr/lib/cups/driver/driverless
|   |-- http -> ipp
|   |-- implicitclass
|   |-- ipp
|   |-- lpd
|   |-- parallel
|   |-- rasterprint
|   |-- serial
|   |-- snmp
|   |-- socket
|   `-- usb
|-- cgi-bin
|   |-- admin.cgi
|   |-- classes.cgi
|   |-- help.cgi
|   |-- jobs.cgi
|   `-- printers.cgi
|-- daemon
|   |-- cups-deviced
|   |-- cups-driverd
|   |-- cups-exec
|   `-- cups-lpd
|-- driver
|   `-- driverless
|-- filter
|   |-- bannertopdf
|   |-- brftoembosser
|   |-- brftopagedbrf
|   |-- cgmtopdf -> vectortopdf
|   |-- cmxtopdf -> vectortopdf
|   |-- commandtoescpx
|   |-- commandtopclx
|   |-- commandtops
|   |-- emftopdf -> vectortopdf
|   |-- gziptoany
|   |-- imagetobrf
|   |-- imagetopdf
|   |-- imagetops
|   |-- imagetoraster
|   |-- imagetoubrl -> imagetobrf
|   |-- imageubrltoindexv3
|   |-- imageubrltoindexv4
|   |-- musicxmltobrf
|   |-- pdftopdf
|   |-- pdftops
|   |-- pdftoraster
|   |-- pstops
|   |-- rastertodymo -> rastertolabel
|   |-- rastertoepson
|   |-- rastertoescpx
|   |-- rastertohp
|   |-- rastertolabel
|   |-- rastertopclm
|   |-- rastertopclx
|   |-- rastertopdf
|   |-- rastertops
|   |-- rastertopwg
|   |-- svgtopdf -> vectortopdf
|   |-- sys5ippprinter
|   |-- textbrftoindexv3
|   |-- textbrftoindexv4 -> textbrftoindexv3
|   |-- texttobrf
|   |-- texttopdf
|   |-- texttops
|   |-- texttotext
|   |-- vectortobrf
|   |-- vectortopdf
|   |-- vectortoubrl -> vectortobrf
|   |-- wmftopdf -> vectortopdf
|   `-- xfigtopdf -> vectortopdf
|-- monitor
|   |-- bcp
|   `-- tbcp
`-- notifier
    |-- dbus
    |-- mailto
    `-- rss
```
```
/usr/share/cups
|-- banners
|   |-- classified
|   |-- confidential
|   |-- form
|   |-- secret
|   |-- standard
|   |-- topsecret
|   `-- unclassified
|-- braille
|   |-- cups-braille.sh
|   |-- index.sh
|   |-- indexv3.sh
|   `-- indexv4.sh
|-- charsets
|   |-- pdf.utf-8 -> pdf.utf-8.simple
|   |-- pdf.utf-8.heavy
|   `-- pdf.utf-8.simple
|-- data
|   |-- classified.pdf
|   |-- confidential.pdf
|   |-- default-testpage.pdf
|   |-- default.pdf
|   |-- form_english.pdf
|   |-- form_english_in.odt
|   |-- form_russian.pdf
|   |-- form_russian_in.odt
|   |-- secret.pdf
|   |-- standard.pdf
|   |-- testprint
|   |-- topsecret.pdf
|   `-- unclassified.pdf
|-- drv
|   |-- cupsfilters.drv
|   |-- generic-brf.drv
|   |-- generic-ubrl.drv
|   |-- indexv3.drv
|   |-- indexv4.drv
|   |-- sample.drv
|   `-- spi.drv
|-- examples
|   |-- color.drv
|   |-- constraint.drv
|   |-- custom.drv
|   |-- grouping.drv
|   |-- laserjet-basic.drv
|   |-- laserjet-pjl.drv
|   |-- minimum.drv
|   |-- postscript.drv
|   |-- r300-basic.drv
|   |-- r300-colorman.drv
|   `-- r300-remote.drv
|-- mime
|   |-- braille.convs
|   |-- braille.types
|   |-- cupsfilters-poppler.convs
|   |-- cupsfilters.convs
|   |-- cupsfilters.types
|   |-- mime.convs
|   `-- mime.types
|-- model
|-- ppdc
|   |-- braille.defs
|   |-- epson.h
|   |-- escp.h
|   |-- font.defs
|   |-- fr-braille.po
|   |-- hp.h
|   |-- imagemagick.defs
|   |-- index.defs
|   |-- label.h
|   |-- liblouis.defs
|   |-- liblouis1.defs
|   |-- liblouis2.defs
|   |-- liblouis3.defs
|   |-- liblouis4.defs
|   |-- media-braille.defs
|   |-- media.defs
|   |-- pcl.h
|   `-- raster.defs
|-- profiles
`-- templates
    `-- users.tmpl

```
