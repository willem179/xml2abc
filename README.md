[more music software](https://wim.vree.org/svgParse/index.html)

### xml2abc

_xml2abc_ is a command line utility that translates MusicXML into [ABC](http://abcnotation.com/) notation.

Parts of MusicXML that lie beyond the scope of the ABC standard 2.1 are not translated. For example: specific positioning information, slurs spanning staves.

The program translates all examples in the set from [MakeMusic](http://www.musicxml.com/music-in-musicxml/example-set/).

All non standard extensions of ABC supported by [abc2xml.html](https://wim.vree.org/svgParse/abc2xml.html) are also supported by _xml2abc_. For instance, tablature and percussion scores can be translated from MusicXML into ABC and back to MusicXML, without significant loss of information.

### Usage:

\> python xml2abc.py \[-h\] \[-u\] \[-m\] \[-c C\] \[-d D\] \[-v V\] \[-n CPL\] \[-b BPL\] \[-o DIR\] \[-x\] \[-p FMT\] \[-t\] \[-s\] file1 \[file2 ...\]

Translates all .xml files (MusicXML) or compressed .mxl files in the file list to ABC-notation. Output goes to stdout unless the -o option is given. Wildcards in file names are expanded.

-   Option **\-h** prints help message with explanation of the options.
-   Option **\-u** turns off repeat translation and unfolds simple repeats instead.
-   Option **\-m M** When **M**\=1 only _%%MIDI program_ commands are output. For a percussion voice also _%%MIDI channel_ and _%%MIDI drummap_ statements are added. When **M**\=2 all supported midi commands are output: _%%MIDI channel_, _%%MIDI program_, _%%MIDI contol 7_, _%%MIDI control 10_ and _%%MIDI drummap_ for a percussion voice. The default is **M**\=0, which outputs no midi commands.
-   Option **\-c C** sets the filter level of the credit filter. This filter tries to eliminate redundant T: fields. The level C goes from 0 (default) to 6 (no filtering). A higher level does less filtering. The default filters as much as possible and only includes the xml credit elements if no title of movement-title is found. Level 6 does no filtering and includes all xml credit elements as T: fields.
-   Option **\-d D** sets the unit length for the ABC output to L:1/**D**. This overrides the computation of the optimal unit length per voice.
-   Option **\-v V**. The default (V=0) translates volta backets in all voices. V=1 prevents abcm2ps to write volta brackets on all but the first voice. (A %%repbra 0 command is added to each voice that hides its volta's.) When V=2 abcm2ps only typesets volta brackets on the first voice _of each xml-part_. When V=3 the volta brackets are only translated for the first abc voice, which has the same effect on the output of abcm2ps as V=1, but the abc code is not suited for abc2midi.
-   Option **\-n CPL** sets the maximum line length for ABC output to **CPL** characters. The default is 100 chars per line. An integer number of bars, at least one, is always output. How many bars fit into the maximum line length is calculated from the note line, not from the lyric lines. Line- and page breaks in MusicXML are translated to '$' after the previous barline \[or volta number\]
-   Option **\-b BPL** sets the number of bars per line. Every line of ABC will have **BPL** bars (measures). When both -n and -b options are given -b is ignored.
-   Option **\-o DIR** translates every .xml file to a separate .abc file with the same name into directory **DIR**. For example, _\-o._ puts all abc files into the same directory where the input files reside.
-   Option **\-t** translates percussion (perc) and tablature (tab) staves to ABC code using _%%map_ and _%%voicemap_. It also prepends a _%%beginsvg ... %%endsvg_ section with definitions for noteheads.  
    For percussion only the following MusicXML noteheads are supported: _x_, _circle-x_, _square_, _triangle_ and _diamond_.  
    The tablature translation can map one note (one ABC pitch) to at most 4 different strings (per voice). If you use more strings for one note (in one voice), it will reject those mappings. Mapping of one note to three different strings is abc2midi compatible.  
    See [tutorial](https://wim.vree.org/js2/tabDrumDoc.html) for more explanation and examples.
-   Option **\-s** Shift note heads in a tab staff 3 units to the left. This will draw the note heads straight above the stems. However, ties and dots with seem shifted too much to the right. This option only takes effect when also _\-t_ is present.
-   Option **\-x** disables translation of xml line breaks. No abc score line breaks ($) will be output.
-   Option **\--v1** allocates all XML staff directions to the first voice of the staff. Normally a staff direction is allocated to the voice of the next note after the direction (if any).
-   Option **\--noped** disables translation of xml pedal elements.
-   Option **\-p FMT**, FMT=scale,pageheight,pagewidth,leftmargin,rightmargin,topmargin,botmargin. The specified pageformat parameters are included in the generated abc code as pseudo instructions (%%scale, %%pageheight etc.) For example: _\-p 0.7,25,15,1.2,1.2,1.2,1.2_ sets the scale to 0.7, the page height to 25 cm, the page width to 15 cm and all margins to 1.2 cm. After -p should follow 7 (floating point) numbers separated by comma's and without any spaces! Alle measures are in centimeter (cm). The scale value is the same as used by abcm2ps, i.e. a floating point number somewhere around 1.0
-   Option **\-pf** (this is option -p without number list) Enables the translation of xml page format specifications into abc page format instructions (%%pageheight etc).
-   Option **\-i** tells _xml2abc_ to read XML data from standard input. When writing the an output file with option **\-o** the name of the output file is _stdin.xml_.
-   Option **\--nbr** disables translation to broken rhythms. Normally xml2abc tries to write broken rythms whenever possible. This option disables that behaviour
-   Option **\--rbm** re-beams all measures. Only meant for files that completely lack any beaming.
-   Option **\--fin=N** selects the rule set for microtonal accidentals. Some well known score editors behave differently when microtonal accidentals are present in the score. _xml2abc_ uses different translation rules, depending on which score editor was used to make the xml file When microtonal accidentals are present in the score, _xml2abc_ searches for the identification of the encoding software. The following encoders are recognized: _abc2xml_, _burak uyar_, _musescore_ and _finale_. When no match is found _musescore_ is assumed. With option --fin one can override the automatic detection:  
    _abc2xml_: --fin=0  
    _burak uyar_: --fin=1  
    _musescore_: --fin=2 (default)  
    _finale_: --fin=3  
    
-   Option **\--temp=N** selects the temperament for microtonal accidentals. When microtonal accidentals are present in the score a corresponfing tuning is aufomatically selected. Only EDO-53 and EDO-36 are implemented. EDO-36 is used for the up/down accidentals in MusicXML and EDO-53 for the makam accidentals. When a mixture of these accidentals is detected, EDO-53 is selected and the up/down accidentals are mapped to the closest makam variant.  
    \--temp=0 => only map up/down accidentals when they occur mixed with makam accidentals (default)  
    \--temp=1 => always map up/down accidentals, even when not mixed with makam accidentals (i.e. a pure EDO-36 file)  
    \--temp=2 => up/down accidentals are translated to fractional ABC accidentals, and can be mixed with (whole number) makam accidentals in ABC. For instance, <natural-up> is translated to ^1/3 and <sharp-down> to ^2/3, but <quarter-sharp> is translated to ^1, and <sharp> to ^4.

For more info see: [abc2xml.html](https://wim.vree.org/svgParse/xml2abc.html)  
Bug reports, remarks to:![](https://wim.vree.org/svgParse/index_email.png)
