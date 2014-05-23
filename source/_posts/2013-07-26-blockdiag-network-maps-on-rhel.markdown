---
author: mhbarr
comments: true
date: 2013-07-26 15:50:15+00:00
layout: post
slug: blockdiag-network-maps-on-rhel
title: BlockDiag Network maps on RHEL
wordpress_id: 29
---

To install blockdiag:

    yum install python-reportlab python-imaging
    easy_install "blockdiag[pdf]"

Generate a pdf

    nwdiag -Tpdf routing.diag -f /usr/share/fonts/dejavu/DejaVuSans.ttf

Generate a SVG

    nwdiag -Tsvg routing.diag

Generate a png.  (Not as pretty, actually.)

    nwdiag routing.diag
