# Penelope

**Penelope** is a multi-tool for creating, editing and converting dictionaries, especially for eReader devices.

* Version: 2.0.2
* Date: 2015-04-04
* Developer: [Alberto Pettarin](http://www.albertopettarin.it/) ([contact](http://www.albertopettarin.it/contact.html))
* License: the MIT License (MIT), see LICENSE.md

With the current version you can:

* convert a dictionary FROM/TO the following formats:
    * Bookeen Cybook Odyssey (R/W)
    * Kobo (R index only, W unencrypted/unobfuscated only)
    * StarDict (R/W)
    * XML (R/W)
    * CSV (R/W)
* merge more dictionaries (of the same type) into a single dictionary
* merge more definitions for the same index word
* define your own parser for each word/definition
* define your own collation function when outputting to Bookeen Cybook Odyssey format
* generate an EPUB file containing the index of a given dictionary (e.g., to cope with the lack of a search function on your eReader)


### Important updates

* 2014-07-30 Please note that Penelope needs substantial code refactoring. Many people have asked for PRC/MOBI support. Unfortunately, I no longer have time to develop or maintain Penelope. **Please fork and improve.**
* 2014-06-30 I moved Penelope to GitHub, and released it under the MIT License, with the version code v2.0.0.


## Usage

```
$ python penelope.py -p <prefix list> -f <language_from> -t <language_to> [OPTIONS]

Required arguments:
 -p <prefix list>           : list of the dictionaries to be merged/converted (without extension, comma separated)
 -f <language_from>         : ISO 639-1 code language_from of the dictionary to be converted
 -t <language_to>           : ISO 639-1 code language_to of the dictionary to be converted

Optional arguments:
 -d                         : enable debug mode and do not delete temporary files
 -h                         : print this usage message and exit
 -i                         : ignore word case while building the dictionary index
 -z                         : create the .install zip file containing the dictionary and the index
 --sd                       : input dictionary in StarDict format (default)
 --odyssey                  : input dictionary in Bookeen Cybook Odyssey format
 --xml                      : input dictionary in XML format
 --kobo                     : input dictionary in Kobo format (reads the index only!)
 --csv                      : input dictionary in CSV format
 --output-odyssey           : output dictionary in Bookeen Cybook Odyssey format (default)
 --output-sd                : output dictionary in StarDict format
 --output-xml               : output dictionary in XML format
 --output-kobo              : output dictionary in Kobo format
 --output-csv               : output dictionary in CSV format
 --output-epub              : output EPUB file containing the index of the input dictionary
 --merge-definitions        : merge definitions for the same index word
 --merge-separator <string> : use <string> as separator between merged definitions (default: " ")
 --title <string>           : set the title string shown on the Odyssey screen to <string>
 --license <string>         : set the license string to <string>
 --copyright <string>       : set the copyright string to <string>
 --description <string>     : set the description string to <string>
 --year <string>            : set the year string to <string>
 --parser <parser.py>       : use <parser.py> to parse the input dictionary
 --collation <coll.py>      : use <coll.py> as collation function when outputting in Bookeen Cybook Odyssey format
 --fs <string>              : use <string> as CSV field separator, escaping ASCII sequences (default: \t)
 --ls <string>              : use <string> as CSV line separator, escaping ASCII sequences (default: \n)

Examples:
$ python penelope.py -h
$ python penelope.py           -p foo -f en -t en
$ python penelope.py           -p bar -f en -t it
$ python penelope.py           -p "bar,foo,zam" -f en -t it
$ python penelope.py --xml     -p foo -f en -t en
$ python penelope.py --xml     -p foo -f en -t en --output-sd
$ python penelope.py           -p bar -f en -t it --output-kobo
$ python penelope.py           -p bar -f en -t it --output-xml -i
$ python penelope.py --kobo    -p bar -f it -t it --output-epub
$ python penelope.py --odyssey -p bar -f en -t en --output-epub
$ python penelope.py           -p bar -f en -t it --title "My EN->IT dictionary" --year 2012 --license "CC-BY-NC-SA 3.0"
$ python penelope.py           -p foo -f en -t en --parser foo_parser.py --title "Custom EN dictionary"
$ python penelope.py           -p foo -f en -t en --collation custom_collation.py
$ python penelope.py --xml     -p foo -f en -t en --output-csv --fs "\t\t" --ls "\n" 
```

You can find [ISO 639-1 codes here](http://en.wikipedia.org/wiki/List_of_ISO_639-1_codes).

You need `Python`, either version 2.x or 3.x, installed on your system to run Penelope.

You might need `dictzip` installed in your system to read from/write to StarDict dictionaries.

If you want to read from/write to Kobo format,
you need a compiled version of [`MARISA`](https://code.google.com/p/marisa-trie/).
In case, you must modify the value of variables `MARISA_BUILD_PATH` and `MARISA_REVERSE_LOOKUP_PATH`
in `penelope.py` (Python 2.x) or `penelope3.py` (Python 3.x),
making it pointing to the `marisa-build` and `marisa-reverse-lookup`
executables (see the corresponding comments in the source code). 

Old Web page about Penelope on my personal Web site:
http://www.albertopettarin.it/penelope.html

### CSV format

Penelope can read/write CSV-like files:

* one line for each (word, definition) pair
* each line must end with the same line separator (default: `\n`)
* each `word` and `definition` must be separated by the same field separator (default: `\t`)

Note that you can change the default line and field separators
to any arbitrary string, by using the `--ls` and `--fs` switches, respectively.
The string might contain ASCII escapes, for example `\t\t\t` represents three tab characters.

### XML format

The `XML` format read/written by Penelope is defined by
[dictionary.dtd](https://github.com/pettarin/penelope/blob/master/src/dictionary.dtd).

See [test.xml](https://github.com/pettarin/penelope/blob/master/src/test.xml)
for an example.


## Installing the Dictionaries

### Bookeen Odyssey Devices

For example, suppose you want to use an IT -> EN dictionary.

1. On your PC, produce/download the IT -> EN dictionary files `it-en.dict` and `it-en.dict.idx`.
2. Connect your Odyssey device to your PC via the USB cable.
3. Using your file manager, copy the two files `it-en.dict` and `it-en.dict.idx`
from your PC into the `Dictionaries/` directory on your Odyssey device.
4.  Reboot your Odyssey, open a book in Italian and select a word: the definition in English should appear.
(For this test, select a common word so you are sure it is present in the dictionary!)

Note that the Bookeen dictionary software will select the dictionary
to use by reading the `dc:language` metadata of your eBook.
Make sure your eBooks have the proper `dc:language` metadata,
otherwise the correct dictionary might not be loaded.


### Kobo Devices

At the time of this writing (2015-04-04), Kobo devices will load dictionaries
only if the files have a file name of an official Kobo dictionaries, which are:

* `dicthtml.zip` (EN)
* `dicthtml-de.zip` (DE), `dicthtml-de-en.zip` (DE -> EN), `dicthtml-en-de.zip` (EN -> DE),
* `dicthtml-es.zip` (ES), `dicthtml-es-en.zip` (ES -> EN), `dicthtml-en-es.zip` (EN -> ES),
* `dicthtml-fr.zip` (FR), `dicthtml-fr-en.zip` (FR -> EN), `dicthtml-en-fr.zip` (EN -> FR),
* `dicthtml-it.zip` (IT), `dicthtml-it-en.zip` (IT -> EN), `dicthtml-en-it.zip` (EN -> IT),
* `dicthtml-nl.zip` (NL)
* `dicthtml-ja.zip` (JA), `dicthtml-en-ja.zip` (EN -> JA),
* `dicthtml-pt.zip` (PT), `dicthtml-pt-en.zip` (PT -> EN), `dicthtml-en-pt.zip` (EN -> PT)

(see [this MobileRead thread](http://www.mobileread.com/forums/showthread.php?t=196931))

Hence, if you want to install a custom dictionary produced with Penelope,
you must choose to overwrite one of the official Kobo dictionaries,
effectively loosing the possibility of using the latter.

For example, suppose you want to use a Polish dictionary (`dicthtml-pl.zip`),
while you are not interested in using the official Portuguese one (`dicthtml-pt.zip`).

1. On your PC, produce/download the Polish dictionary `dicthtml-pl.zip`.
2. In your Kobo device, go to the settings and activate the Portuguese dictionary.
3. Connect your Kobo device to your PC via the USB cable.
4. Using your file manager, copy `dicthtml-pl.zip`
from your PC into the `.kobo/dict/` directory on your Kobo device.
(Note that `.kobo` is a hidden directory: you might need to enable
the "show hidden files/directories" setting of your file manager.)
5. Rename `dicthtml-pl.zip` into `dicthtml-pt.zip`.
6. Reboot your Kobo, open a book in Polish and select a word: the definition should appear.
(For this test, select a common word so you are sure it is present in the dictionary!)

Note that if you update the firmware of your Kobo,
the custom dictionaries might be overwritten with the official ones.
Hence, keep a backup copy of your custom dictionaries in a safe place,
e.g. your PC or a SD card.

You can find a list of custom dictionaries, mostly done with Penelope, in
[this MobileRead thread](http://www.mobileread.com/forums/showthread.php?t=232883).



## License

**Penelope** is released under the MIT License since version 2.0.0 (2014-06-30).

Previous versions, hosted in a [Google Code repo](http://code.google.com/p/penelope-dictionary-converter/),
were released under the GNU GPL 3 License.


## Limitations and Missing Features 

* No support for PRC/MOBI dictionaries 
* Input files are assumed to be Unicode UTF-8 encoded


## Acknowledgments 

Many thanks to:

* _uwelovesdonna_ for contributing ideas for improving the code and for setting up many pages of the project wiki;
* _Jens Sadowski_ for pointing out a bug with Unicode file names and for suggesting using multiset `dict()` instead of set `dict()`;
* _oldnat_ for pointing out a bug under Windows and Python 3;
* _Wolfgang Miller-Reichling_ for providing the code for reading CSV dictionaries;
* _branok_ for providing the idea and initial code for German collation function;
* _pal_ for suggesting passing `-l` switch to `MARISA_BUILD`;
* _Lukas Brückner_ for suggesting escaping `& < >` when outputting in XML format;
* _Stephan Lichtenhagen_ for suggesting forcing UTF-8 encoding on Python 3;
* _niconavarrete_ for pointing out the dependency from $CWD (issue #1), solved in v2.0.1;
* _elchamaco_ for providing a StarDict dictionary with a `.syn` file for testing.

[![Analytics](https://ga-beacon.appspot.com/UA-52776738-1/penelope)](http://www.albertopettarin.it)
