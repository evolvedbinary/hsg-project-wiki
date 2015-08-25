Apache Tika is a tool for converting—or more precisely, *extracting content* from—Word, PDF, and other Office files to XML. (See the full list of supported formats on the [Apache Tika](https://tika.apache.org/) site, under "Supported Formats.") Tika takes files, extracts the content of the files (including any metadata it can find in the file, such as date created, author, etc.), and outputs the result as XHTML (a well-formed XML version of HTML).

There are two ways we have for running Tika: (1) using eXist and (2) using the command line. The eXist method is most suitable when the files you're working with are already stored in eXist, or when you want to manipulate the results with XQuery; for usage directions, see eXist's documentation, [Content Extraction](http://exist-db.org/exist/apps/doc/contentextraction.xml). The command line method of running Tika is convenient for quickly converting files stored on your hard drive to XML, previewing the output in the browser, editing them with oXygen, etc.

## Command Line Setup

(Note: These directions assume you're using a current version of Mac OS X.)

1. Open the Terminal (in Finder, from the "Go" menu, select "Utilities")
1. Install [Homebrew](http://brew.sh/), if you haven't already. If you've previously installed Homebrew, update Homebrew with this command: `brew update`
1. Install Tika with this command: `brew install tika`

## Usage

To convert a file, my_file.docx, to XML with Tika, use: 

    tika -x my_file.docx > my_file.xml

This says, "Tika, convert `my_file.docx` to XML, and put the results in a new file, `my_file.xml`." What you call the resulting file is up to you.

Note: Unless you specify a specific path for the source file and the resulting file, Tika assumes the source file is in the Terminal's "current directory", and it places the resulting file in that same directory. To point Tika at a file stored in a specific directory, you can drag the file onto the terminal window. Similarly, you can drag a folder into the Terminal to provide Tika with the location for storing the output. You may well end up with a command that looks like this:

    tika -x /Users/joe/Downloads/my_file.pdf > /Users/joe/Desktop/my_file.xml

## Performing OCR on Image Files

To empower Tika to extract text from image files (e.g., JPG, PNG, GIF, TIFF, and PDF), you need to install Tesseract, a free, open source OCR tool. To install Tesseract, enter these commands in the Terminal:

    brew install leptonica --with-libtiff
    brew install tesseract --all-languages

Then run Tika as normal on these image files.

Some PDF files may contain images that need to be OCRed. You'll know this is the case if Tika isn't extracting all of the content that you see in a PDF. A good tool for performing OCR on a PDF is [OCRmyPDF](https://github.com/fritz-hh/OCRmyPDF). To install OCRmyPDF, enter these commands in the Terminal:

    brew install libpng openjpeg jbig2dec qpdf ghostscript python3 libxml2 
    pip3 install --upgrade pip
    pip3 install --upgrade pillow

To run OCRmyPDF on my_file.pdf, enter the following command in the Terminal:

    ocrmypdf my_file.pdf my_file-with-ocr.pdf

When it's finished, you'll have a new, OCRed version of the file, called `my_file-with-ocr.pdf`.  Then you can run Tika on this file to extract the text:

    tika -x my_file-with-ocr.pdf > my_file.xml