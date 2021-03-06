# Gpdf ![.github/workflows/build.yml](https://github.com/billthefarmer/gpdf/workflows/.github/workflows/build.yml/badge.svg)
## GEDCOM genealogy pdf chart creation

Read and parse a GEDCOM format text file into memory and produce a
large format pdf output file. This program was developed and tested
using GEDCOM output from the
[Webtrees](https://www.webtrees.net/index.php/en) geneology
software. It has also been tested with the allged.ged test file which
contains masses of data which is ignored.

The [GEDCOM parser library](http://gedcom-parse.sourceforge.net)
initially tried would only successfully build on 32 bit
linux. [GHOSTS](http://www.nongnu.org/ghosts/users/index.html)
produced similar results. So the parser was written as part of the
program, as the GEDCOM format is almost self documenting once
inspected. This means that the program will only handle ASCII and a
subset of UTF-8 text.

[libHaru ](http://libharu.org) was used to produce the pdf
output. This will build successfully after the build files are
patched. The program is written in generic C, so should work on any
platform that libHaru will build on. To build libHaru on windows
requires zlib and libpng, which can be found in MingW32. To build
libHaru on Mac OSX requires libpng, which can be built and
installed. On Linux, just install zlib and libpng if they are not
already installed. On Linux Mint thats:
```
$ sudo apt install libz-dev libpng-dev
```
MingW32 appears not to contain `getline()`, which is a bit
essential. An implementation was discovered online here:
https://opensource.apple.com/source/cvs/cvs-19/cvs/lib/getline.c. This
is not needed on linux and not on OSX either.

To run on windows you will need to extricate libpng and zlib from
MingW32 and put them in the execution folder with libHaru.
```
Usage: gpdf.exe [-w] [-r <textfile>] [-p pagesize] [-f fontsize] <infile>

  -w - write text file and layout page
  -r - read text file before write
  -p - set page size A0 -- A4
  -f - set font size in points (1/72 inch)
```
The font size defaults to 8 point and the page size to A3. To use the
program, use the -w switch for the initial run like this:
```
$ gpdf -w <infile.ged>
```
This will produce an A3 pdf file with a grid of slots with x, y
positions, and a text file with a list of ids, xrefs, two zeros, the
suggested x position and the name for each individual in the
file. Like this:

![](https://github.com/billthefarmer/billthefarmer.github.io/raw/master/images/gpdf/slots.png)
```
   0        posn suggested
   0  xref  x  y     x      Name
   1  I1    1  1     1      another name /surname/
   2  I2    1  2     1      /Wife/
   3  I3    0  1     0      /Child 1/
   4  I4    0  2     0      /Child 2/
   5  I5    2  1     2      /Father/
   6  I6    2  2     2      /Adoptive mother/
   7  I7    0  3     0      /Child 3/
   8  I8    0  0     1      /2nd Wife/
```
The suggested x position in the file is based on the calculated
generation of the individual based on the number of generations of
ancestors. It won't always be correct, depending on the data.  Fill in
the x, y positions for each individual in the file and run the program
again without the -w switch. This will produce a pdf file with the
chart.

![](https://github.com/billthefarmer/billthefarmer.github.io/raw/master/images/gpdf/allged.png)

You can do test runs with only a few positions filled in to see what
it looks like. Individuals with two zeros for the position won't
appear. The tree below has eleven generations and 108 individuals, with
one generation containing 18 individuals. It would be possible to fit
more people on an A3 page, but doing it without the lines crossing
becomes more difficult.

![](https://github.com/billthefarmer/billthefarmer.github.io/raw/master/images/gpdf/smith.png)
