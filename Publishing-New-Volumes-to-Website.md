If you have the XML for the volume, skip step 2. If you do not have XML for the volume, skip step 4.  

NOTE: These steps assume that you have already received a final pdf for the volume that you have cropped, renamed according to file naming protocol, and uploaded to S3.  

##  1. Edit Volume Metadata
In order to be able to create the shell file, you must first edit the metadata file for the volume and upload it to the localhost. 	
Open the volume’s corresponding volume XML file in hsg-project/repos/frus/bibliography and copy and paste the first two paragraphs of the press release in <summary>. Since this file is not coded in TEI/XML, when you tag paragraphs and text styling you will need to add the tei prefix and namespace to each tag (ex. <tei:p xmlns:tei="http://www.tei-c.org/ns/1.0"> and <tei:hi rend="italic">). You also need to provide any missing information in the file (ISBN, General Editor, publication date, etc.) and correct information already there. 
In order to view the volume metadata and shell file locally, you will need to change the volume’s <publication-status> to “published” and upload the file and the shell file you will create in step 2 to the localhost. Here you can make corrections to the file and reupload to the localhost to view your corrections.  Neither of these files should be uploaded to hsg, however, until you have completed all of the steps below and are ready to publish the volume. 

## 2. Create Shell File
	After you have uploaded the volume metadata file to the localhost, navigate to http://localhost:8080/exist/apps/release/ and click on “Shell File Helper.” On the next page, enter the volume id and click “Generate Shell Files.” A draft shell file for the volume will download to your computer. Open the file in oXygen and save it in hsg-project/repos/frus/volumes. 
	Double check the information that is already populated in the shell file and fill out the div/@xml-id=”pressrelease” portion of the template with:
	1. The publication date
	2. The content paragraphs of the press release
	3. GPO S/N
	4. editor information
When you are finished, upload the file to the localhost and review the volume landing page. 	

## 3. Review the Volume XML

	When you receive the final xml for your volume, you will largely follow the same steps for reviewing the volume that we followed before transferring to GitHub:

	1. Create an issue in GitHub at https://github.com/HistoryAtState/frus and copy and paste the review checklist (found at https://github.com/HistoryAtState/hsg-project/wiki/reviewing-frus-tei) into the issue. 
	2. Rename the final xml file according to our file naming protocol and move the file to hsg-project/repos/frus/volumes and upload to the localhost. You will have to replace the file that is currently there if you published the volume. 
	3. Review the file following the instructions at https://github.com/HistoryAtState/hsg-project/wiki/reviewing-frus-tei for any clarification. 
	4. When you have finished, commit the new file to GitHub and upload to hsg.  

## 4. Generate TIFFs, PNGs, and Ebooks
NOTE: In order to make tiffs and pngs, you need to already have Homebrew, Ghostscript, and Imagemagick installed. If you have already installed these dependencies, skip the steps below. 

	### INSTALL HOMEBREW, GHOSTSCRIPT, IMAGEMAGICK
1. Follow the instructions at http://brew.sh to install Homebrew
2. At the Terminal prompt, paste `brew install imagemagick --with-libtiff` to install Imagemagick
3. Then, paste ` brew install gs` to install Ghostscript. 

	### CREATE TIFFS AND PNGS
If you have the final pdf of the volume, you can make tiffs and pngs while you wait for the final xml. To do this you will need to:
	1. Create a new folder on your desktop and place the final pdf in the folder, ensuring that you have renamed it according to our file naming protocol to match the volume-id. 
	2. Go to Spotlight in the upper-right hand corner of your Mac and search for “Terminal”. Open Terminal. 
	3. Update Homebrew by pasting `brew update` in Terminal, then `brew upgrade`
	3. At the command line, type the following in this order (brackets indicate user action to be performed not typed):

`cd [drag and drop folder you just created in step 1]`

`for file in *.pdf; do FOLDER=`basename -s .pdf $file`; mkdir $FOLDER; cd $FOLDER; mkdir tiff; mkdir medium; mkdir pdf; cd ..; mv $file "$FOLDER/pdf/"; done` 

This should create a parent folder with 3 subfolders named “pdf” “tiff” and “medium”

` for folder in $(find * -maxdepth 0 -type d ); do gs -dBATCH -dNOPAUSE -q -sDEVICE=tiffg4 -r600 "-sOutputFile=$folder/tiff/%04d.tif" "$folder/pdf/$folder.pdf"; done`

This splits the pdf into tiffs that are in the “tiff” folder

` for folder in $(find * -maxdepth 0 -type d ); do mogrify -path "$folder/medium" -format png -density 72 "$folder/tiff/*.tif"; mogrify -resize 'x800' "$folder/medium/*.png"; done` 

This transforms the tiffs into screen resolution pngs that are in the “medium” folder. 

The final step takes significantly longer to complete so be sure that the process is done running before you upload the page images to S3. When this is done running, upload the parent folder with the 3 subfolders to S3 in the directory static.history.state.gov/frus 

	### CREATE EBOOKS
Once you have the final XML for the volume and have reviewed it thoroughly, you are ready to make ebooks. In order to do this you will need to first open both files for the volume in oXygen. (volume file at `hsg-project/repos/frus/volumes` and `hsg-project/repos/frus/bibliography`) Upload both of these files to the localhost.  
	Navigate to http://localhost:8080/exist/apps/release/ and click “EBook Batch Helper.” Be sure the the radio button is set to generate “Both” epub and mobi-bound epubs then click “Generate Ebooks.” 
	In your Downloads folder, you will see a folder entitled “frus-ebooks-[date]” with subfolders “epub” “mobi” and “mobi-bound”. When you see that the Ebook batch conversion has successfully completed at the bottom of the Ebook Batch Helper, open the “epub” subfolder in your Downloads folder. In the folder, you should see a .epub file named with the volume’s frus-id. Open this file and review it in iBooks. If the file is appropriately formatted (check the table of contents, click the hyperlinks for random documents, check headings, lists, and tables, etc.) upload this file to the volume’s folder in S3 and in a new subfolder entitled “ebook”.
	After you have uploaded the .epub of the volume, open the “mobi-bound” subfolder in your Downloads. You will notice that there is another .epub in this folder that has the same name as the file you just uploaded to S3. It is very important that when you are converting this file to a .mobi using Calibre that you use the file in the “mobi-bound” subfolder. If you accidentally use the .epub file in the “epub” subfolder to generate the .mobi, you will notice that all lists in the volume are incorrectly formatted as bulleted lists in the .mobi and you will have to reconvert using the mobi-bound epub. 
	Open Calibre and drag and drop the mobi-bound epub file into the app. It should appear as item number 1 in the center console. Right click on the volume title and click “Convert Books” -> “Convert Individually.”  In the new dialog box that pops up, change “Output format” in the upper right-hand corner of the box from “EPUB” to “MOBI” and click “OK.” When Calibre has finished the conversion, you will find the .mobi in Calibre Library in a subfolder that has the volume’s full name. Rename the .mobi according to our file naming protocol and open it using Kindle for Mac. Review the .mobi as you did the epub and when you have completed the review, upload it to S3 in the same directory as the epub you uploaded above. 
	As with the pdf, in order to ensure that a link to both ebooks appears on the volume landing page, you will need to update the volume’s cache. The instructions for this are below.  


## 5. Update Volume Cache

We are still able to use S3 Cache Helper, but there are several steps that we have to do differently to update the cache. 

You can find S3 Cache helper at http://localhost:8080/exist/apps/release/s3-cache.xq

The first time that you navigate to this page, you will get an error. In order to get it to show up you will need to: 
	1. Open oXygen and navigate to hsg-project/repos/s3/modules and open the file “aws_config_tmpl.xqm” 
	2. Enter the Access Key and Secret Key between the `’ ’` in the file and at the top of the screen click on File -> Save As
	3. In the save screen, save the file as `aws_config.xqm` in repos/s3/modules
	4. Upload this file to the localhost. 
	5. Go back to S3 Cache Helper, click Refresh, and you should see it. 

Before you use S3 Cache Helper, ensure that you are logged into eXide. 
1. Open eXide http://localhost:8080/exist/apps/eXide/index.html
2. In the top navigation bar, click “Login” and enter the localhost credentials you were given during your training. 

In order to use the Cache Helper, enter your volume’s id and press Submit. Whereas before, this automatically updated the cache for us, there are a few extra steps that we have to do in the new workflow. 

1. After you press `Submit` open your localhost in Transmit. Navigate down to apps/s3/cache/static.history.state.gov/frus 



Under /frus, you should have a folder with the name of the volume you just submitted. Inside each of the subfolders, you should have a generated resources file. 

 

2. To update the cache, leave your localhost open on Transmit and on the other side of the app open 1861.history.state.gov and navigate to apps/s3/cache/static.history.state.gov/frus

Both sides should mirror each other like this:

 

3. Move the volume folder from the localhost into the corresponding folder in 1861.history.state.gov



## 6. Create Carousel Entry
As with our previous workflow, in order to update the carousel for the publication of a new volume or quarterly release, you will need to create a new entry and update the display order. The files you need are now located at `hsg-project/carousel/display-order` and `hsg-project/carousel/entries`
	1. Open `display-order.xml` and add your new topic-id at the top of `display-order`, deleting the lowest topic-id so there are no more than 3 in `display-order`
	2. As with shell files, to create a new topic entry, you can either open the most recent topics entry, edit the information for your volume—remembering to change the id to one higher, and save the file as a new entry or your can open a blank XML document and paste in the following to edit and save in the repo:

<?xml version="1.0" encoding="UTF-8"?>
<topic>
    <id>72</id>
    <type>publication</type>
    <title>Now Available: <em>Foreign Relations of the United States</em>, 1977–1980, Volume XXX,
        Public Diplomacy</title>
    <body>This volume documents the public diplomacy efforts of the Jimmy Carter administration. A
        major emphasis of the volume is the role the United States Information Agency played in the formulation and
        implementation of public diplomacy.</body>
    <link>/historicaldocuments/frus1977-80v30</link>
    <image>https://s3.amazonaws.com/static.history.state.gov/frus/frus1977-80v30/covers/frus1977-80v30.jpg</image>
    <image-description>Book cover of <em>Foreign Relations of the United States</em>, 1977–1980, Volume XXX,
        Public Diplomacy</image-description>
    <image-height>600</image-height>
    <image-width>400</image-width>
    <image-source-information>Created by the Office of the Historian</image-source-information>
    <created-by>eckrothsl</created-by>
    <created-datetime>2016-06-07T07:20:59.72-04:00</created-datetime>
    <last-modified-by>eckrothsl</last-modified-by>
    <last-modified-datetime>2016-06-07T07:20:59.72-04:00</last-modified-datetime>
</topic>

Once you have finished editing both files, upload them to localhost and review them one last time. 

