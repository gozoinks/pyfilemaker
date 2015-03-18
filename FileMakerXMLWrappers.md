# Wrappers for FileMaker XML functionality #

## Context of pyFIleMaker ##

In several ways pyFileMaker is a sequell to FX.php, the binding between php and FileMaker since Filemaker 5 through 9, written by Chris Hansen, Chris Adams and Gjermund Gusland Thorsen aka ggt667 (Unicode and other conveniences). The invention of FX.php was mainly to bring FileMaker to one of the most common web platforms; php, but also a to show open source as a competitive alternative to the proprietary layers( or speed bumps if you will ) named Lasso and Tango...

In spirit more than technical descendence pyFileMaker is taking this step further, by giving you the chance to bring your FileMaker data to any python scriptable environment there is; anywhere, our primary use so far has been mail robots, and parsers( PDFs, Excel sheets, various XML formats ), but this will also allow you to make custom iPhone apps in a python environment without having to relay on web interface; in the same fashion as stocks or weather.

The current implementation of pyFileMaker has mainly been written by Petr Pridal aka klokan, with testing performed with colleague Gjermund Gusland Thorsen. It is based on original version of pyFileMaker designed by Pieter Claerhout.

What we are releasing in this package is the "least squares", that we believe others will benefit from. Again high impact, and freedom of speech is encouraged; even though some people might want us to keep this to ourselves, we thoroughly believe in sharing, for new knowledge to prosper on top of the one currently known by the human race.

# List of existing wrappers #

There are several ways to interchange data with FileMaker’s XML RPC
  * XSLT templates
  * Lasso( Proprietary )
  * FX.php
  * FileMaker and php
  * FileMaker API for PHP
  * pyFileMaker


# XSLT templates #

Standardized way to export and import Filemaker data. The FileMaker XML interface can directly interpret XML transformations defined as XSLT. This is perfect for simple tasks.

# Lasso #

Proprietary approach to interchanging Filemaker data with the web.


# FX.php #

Open Source approach to interchanging Filemaker data with the web.

Good documentation, live hands on example http://iviking.org/FX.php/


# FileMaker and PHP #

Open Source approach to interchanging Filemaker data with the web, derived from FX.php? Main change AFAIK is that it comes with alot of documentation, in the form of sample queries in a PDF file, but it’s alot more than FX.php’s 7 pages or whatver, but is it needed? German or Austrian approach? I have not seen any updates to this one last 2 years. However I did find the homepage in the archive: http://www.fm-and-php.info/

# FileMaker API for PHP #

FileMaker’s own php approach to interchanging FileMaker data with the web, using objects instead of arrays,
but with preformatted templates for rapid web development.

Warning: attempt done to cripple the code.


# pyFileMaker #

Open Source approach to interchanging Filemaker data with anything. You are on pages of this project now.