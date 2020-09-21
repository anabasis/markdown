# PYTHON

## CHAPTER03

### Python sys Variables

|Python sys Variables||
|:---|:--|
|argv|Command line args
|builtin_module_names|Linked C modules
|byteorder|Native byte order
|check_ interval|Signal check frequency
|exec_prefix|Root directory
|executable|Name of executable
|exitfunc|Exit function name
|modules|Loaded modules
|path|Search path
|platform|Current platform
|stdin, stdout, stderr|File objects for I/O
|version_info|Python version info
|winver|Version number

### Python sys.argv

```python
sys.argv for the command:
$ python foo.py bar -c qux --h
```

|Python sys.argv||
|:--|:--|
|sys.argv[0]|foo.py
|sys.argv[1]|bar
|sys.argv[2]|-c
|sys.argv[3]|qux
|sys.argv[4]|--h

### Python os Variables

- Registered OS names: " pos ix", " nt"," mac ", " os2 ", " ce", " jav a", " ris cos "

|Python os Variables||
|:--|:--|
|altsep|Alternative sep|
|curdir|Current dir string|
|defpath|Default search path|
|devnull|Path of null device|
|extsep|Extension separator|
|linesep|Line separator|
|name|Name of OS|
|pardir|Parent dir string|
|pathsep|Patch separator|
|sep|Path separator|

### Python Class Special Methods

__new_ _(cls) __lt__ (self, other)
__init __( self, args) __le__ (self, other)
__del_ _(self) __gt__ (self, other)
__repr __( self) __ge__ (self, other)
__str_ _(self) __eq__ (self, other)
__cmp_ _(self, other) __ne__ (self, other)
__inde x__ (self) __nonz ero __( self)
__hash __( self)
__getattr __( self, name)
__getattr ibu te_ _(self, name)
__setattr __( self, name, attr)
__delattr __( self, name)
__call __( self, args, kwargs)

### Python String Methods (cont)
istitle() * title() *
isupper() * transl ate (table)
join() upper() *
ljust( width) zfill( width)
lower() *
Methods marked * are locale dependant for 8-
bit strings.

### Python File Methods
close() readli nes (size)
flush() seek(o ffset)
fileno() tell()
isatty() trunca te( size)
next() write( string)
read(size) writel ine s(list)
readli ne( size)

## Date함수

### Python Datetime Methods
today() fromor din al( ord inal)
now(ti mez one info) combin e(date, time)
utcnow() strpti me( date, format)
fromti mes tam p(t ime stamp)
utcfro mti mes tam p(t ime stamp)

### Python Time Methods
replace() utcoff set()
isofor mat() dst()
__str__() tzname()
strfti me( format)

### Python Date Formatting
%a Abbrev iated weekday (Sun)
%A Weekday (Sunday)
%b Abbrev iated month name (Jan)
%B Month name (January)
%c Date and time
%d Day (leading zeros) (01 to 31)
%H 24 hour (leading zeros) (00 to 23)
%I 12 hour (leading zeros) (01 to 12)
%j Day of year (001 to 366)
%m Month (01 to 12)
%M Minute (00 to 59)
%p AM or PM
%S Second (00 to 61⁴)
%U Week number¹ (00 to 53)
%w Weekday² (0 to 6)
%W Week number³ (00 to 53)
%x Date
%X Time
%y Year without century (00 to 99)
%Y Year (2008)
%Z Time zone (GMT)
%% A literal " %" character (%)
¹ Sunday as start of week. All days in a new year preceding the first Sunday are considered to be in week 0.
² 0 is Sunday, 6 is Saturday.
³ Monday as start of week. All days in a new year preceding the first Monday are considered to be in week 0.
⁴ This is not a mistake. Range takes account of leap and double-leap seconds.