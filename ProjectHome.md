# PyFileMaker - Python Interface for FileMaker #

PyFileMaker is a set of Python modules that makes it easy to access and modify data stored in a FileMaker Server database. It uses XML data interchange trough Custom Web Publishing functionality of the server. It acts as Object Relational Mapper for existing FileMaker database.

You can use it to query a FileMaker database, but you can also use it to add data to a FileMaker database, you can even use it to delete records and execute FileMaker scripts.

It is one of the [available wrappers for FileMaker's Custom Web Publishing](FileMakerXMLWrappers.md) XML functionality, which is described in the document
[FileMaker Custom Web Publishing Guide (PDF)](http://www.google.com/search?q=filemaker%20custom%20web%20publishing%20guide&as_filetype=pdf&btnI=3564)

This interface is very practical for integration of your FileMaker database with existing enterprise systems.
We used this interface for our customers for implementation of:

  * **SOAP/XML-RPC/WEB data interchange** for B2B gateways.

  * **Email robots** - automatically receiving and submitting documents with suppliers, where data are both generated from or inserted into the the FileMaker solution’s Layout of your desire.

  * **Generation of MS Word/Excel documents, forms and questionaries**, with cell values filled from FileMaker, with custom design[s](s.md) and logo[s](s.md) and/or other pictograms, with equations and locked cells, etc.

  * **Generation of printer-ready documents** (for example Business Cards) based on templates. Result is in formats like PDF/PostScript/SVG.

  * **SMS gateway** - when you can use mobile SMS as another gateway to insert/query data in your FileMaker database.

  * **Fast data interchange for closed proprietary applications**, which simulates key strokes to insert data from your FileMaker business solution into other systems.

  * **Synchronization with Google Calendar/iCal** and similar services on the server side of your database. No need to install any plugins to FileMaker client hosts.

  * **Google App Engine**: Nowadays you can even use PyFileMaker for both way data interchange between your FileMaker database and Google BigTable cluster data storage. You can also host your python FileMaker application on the freely available service Google App Engine.

We provide commercial support, customization, and we are able to offer development of your business solutions. Feel free to contact the developers of this project. Don’t let your prejudice let you down, we are here to help you.

# Requirements #

In order to use PyFileMaker, you will need to have the following software
installed on your computer:

  * Python 2.4+ (running on a Linux or other UNIXes, on Mac OS X, Windows, ...)
  * Database on a FileMaker Server Advanced 7, 8, 8.5 and 9.0 with Custom Web Publishing (for XML access).

The module was tested on Mac OS X and on Linux (Debian, Ubuntu) as well as on Windows XP, NT4 and 2000. It should run on any python supported platform.

Older version (1.2) even supports FileMaker Pro 6 with the WebCampanion and XML enabled.

# Instalation #

The latest releases are always available on the [Python Cheese Shop](http://pypi.python.org/pypi/PyFileMaker/), and is installable with [easy\_install](http://peak.telecommunity.com/DevCenter/EasyInstall).

You can install the latest release with:

`easy_install -U PyFileMaker`

In case you need an older version or source tarball or zip check the [Downloads section](http://code.google.com/p/pyfilemaker/downloads/list).

Latest development is in the [Source section](http://code.google.com/p/pyfilemaker/source/checkout) available trough [Subversion Repository](http://code.google.com/p/pyfilemaker/source/browse/trunk).

# Screenshot #

![![](http://pyfilemaker.googlecode.com/svn/wiki/screnshot-small.jpg)](http://pyfilemaker.googlecode.com/svn/wiki/screnshot.jpg)

PyFileMaker is a python module, so on the picture you see an example of interactive session in ipython together with live changes in the FileMaker Database (the second window).

# Usage of PyFileMaker #

PyFileMaker module is designed for both script and interactive use.
Any command used during interactive session is possible to type in a script as well.

## Short introduction by Klokan ##

run python interactively (or better run ipython):

`ipython`


### Selection of the layout in a database ###

```
>> from PyFileMaker import FMServer

>> fm = FMServer('login:password@filemaker.domain.com')
>> fm.
```
[press Tab to see available methods and variables](and.md)
```
>> help fm.getDbNames
```
[help for the method](displays.md)
```
>> fm.getDbNames()
['dbname','anoterdatabase']
>> fm.setDb('dbname')

>> fm.getLayoutNames()
['layoutname','anotherlayout']
>> fm.setLayout('layoutname')
```

You can also type directly:
```
>> fm = FMServer('login:password@filemaker.domain.com','dbname','layoutname')
```

### List fieldnames from active layout ###
```
>> fm.doView()
['column1', 'column2']
```

### Find records ###
```
>> fm.doFind(column1='abc')
<FMProResult instance with 2 records>
[column1='abcdef'
column2'='some data'
RECORDID=1,
column1='abc'
column2='another data'
RECORDID=2]
```

You've got list of 2 records, usually you need to work only with one record.

```
>> a = fm.doFind(column1='abc')
>> len(a)
2
>> r = a[0]
>> r.
```
[Tab for available layout variables or for completition of variable name](press.md)

```
>> r.column1
'abcdef'
>> r['column1']
'abcdef'
>> print r.column1
abcdef
>> r.column.related
'content'

>> fm.doFind( column1='abc', column__related='abc', LOP='OR', SKIP=1, MAX=1)
```

Get latest record if documentID field is autoincremented during insertion in FileMaker
```
>> fm.doFind( SORT=['documentID':'<'], MAX=1)
```
Or more low level access
```
>> fm.doFind( {'documentID.op':'lt', '-max':1})
```
Any combination of attributes is allowed...

BTW For query empty record
```
>> fm.doFind( column1='==')
```

### Creating records ###

New records can be specified as arguments of doNew() like:
```
>>> fm.doNew(column1='newvalue', column2='old')
>>> fm.doNew({'column':'newvalue','column2':'old'})
```

### Editing records ###

You can just change an attribute inside of previosly returned record.

```
>> r = fm.doFindAny()[0]
>> r.column = 'NEWVALUE'
>> fm.doEdit(r)
```

This will update only changed column 'column' in the record.
You can use changed old record in other functions too - doDup, doNew, doDelete.

doFind(r) will find record with the same RECORDID - it is useful in case the record is changed on the server side meanwhile (then it will have a new MODID).

### Templates ###

The structure of returned data is suitable for use with serveral python template engines.
Templates can be written quite easily because of the object data model with correct attributes.

An example with Cheetah Template:

```
import Cheetah.Template
t = Cheetah.Template.Template('''
Document Template
-----------------
DocumentID: $documentID
DocumentType: $DocumentType.documentType

Item descriptions:
#for $l in $DocumentLine
- $l.description
#end for      
''', searchList=[r[0]])
```

### Debugging connection ###

Best way howto debug what's wrong:
```
>> fm._debug = True
```
then check printed url request by external tools (like curl, xmlstarlet):

`$ curl 'http://test:test@filemaker.domain.com:80/fmi/xml/fmresultset.xml?-db=test&-layout=test&-findall' | xmlstarlet fo`

## Older versions: ##

PyFileMaker 1.2 and 2.0 was written by Pieter Claerhout from Belgium.
PyFileMaker 2.5 is written by Klokan Petr Pridal from Czech Republic, with advices from Gjermund Gusland Thorsen, an independent consultant for FileMaker and workflow management from Norway.

Old project website is at:
http://www.yellowduck.be/filemaker

## Alternatives: ##

There is a lightweight module for accessing FileMaker server XML interface from Python written by Christoph Gohlke, and available at his [home page](http://www.lfd.uci.edu/~gohlke/code/fmkr.py.html), it's API is based on fx.php.

On a Mac there is also possibility to script FileMaker application by the Apple Script Interface - also from python scripts using appscript module. More info if you type into google some phrases like
[python appscript filemaker](http://www.google.com/search?q=python+appscript+filemaker) or [macpython filemaker](http://www.google.com/search?q=macpython+filemaker).
Something similar is is possible under Windows trough FileMaker ActiveX.
This interfaces allows mainly simple batch conversion of data using native functionality of the FileMaker application.