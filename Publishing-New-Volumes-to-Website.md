NOTE: These steps assume that you have already received a final PDF for the volume that you have cropped, renamed according to FRUS file naming conventions, and uploaded to S3.  

##  1. Edit Volume Metadata
In order to be able to create the shell file, you must first edit the metadata file for the volume and upload it to the localhost. 
	
Open the volume’s corresponding volume XML file in `hsg-project/repos/frus/bibliography` and copy and paste the first two paragraphs of the press release in `<summary>`. Since this file is not coded in TEI XML, when you tag paragraphs and text styling you will need to add the TEI prefix and namespace to each tag (ex. `<tei:p xmlns:tei="http://www.tei-c.org/ns/1.0"> and <tei:hi rend="italic">`). You also need to provide any missing information in the file (ISBN, General Editor, publication date, etc.) and correct information already there. 

In order to view the volume metadata and shell file locally, you will need to change the volume’s `<publication-status>` to “published” and upload the file and the shell file you will create in step 2 to localhost. Here you can make corrections to the file and re-upload to localhost to view your corrections. Neither of these files should be uploaded to hsg, however, until you have completed all of the steps below and are ready to publish the volume. 

---

**Do you have the XML for the volume?**

- [ ] If YES, skip step 2 and proceed to **[step 3](#3-review-the-volume-xml)**.

- [ ] If NO, proceed to **[step 2](#2-create-shell-file)**.

---

## 2. Create Shell File
After you have uploaded the volume metadata file to the localhost, navigate to the Release app at <http://localhost:8080/exist/apps/release/> and click on `Shell File Helper.` On the next page, enter the volume id and click `Generate Shell Files.` A draft shell file for the volume will download to your computer. Open the file in oXygen and save it in `hsg-project/repos/frus/volumes.` 

Double check the information that is already populated in the shell file and fill out the `div/@xml:id=”pressrelease”` portion of the template with:

1. The publication date
2. The content paragraphs of the press release
3. GPO S/N
4. Editor information

When you are finished, upload the file to the localhost and review the volume landing page.	

## 3. Review the Volume XML

When you receive the final XML for your volume, you will largely follow the same steps for reviewing the volume that we followed before we moved to GitHub:

1. Create an "issue" in GitHub at <https://github.com/HistoryAtState/frus/issues> and copy and paste the review checklist (found at [Reviewing FRUS TEI](https://github.com/HistoryAtState/hsg-project/wiki/reviewing-frus-tei)) into the issue. 
2. Rename the final XML file according to our file naming conventions, move the file to `hsg-project/repos/frus/volumes`, and upload it to localhost. You will have to replace the file that is currently there if you published the volume. 
3. Review the file following the instructions at [Reviewing FRUS TEI](https://github.com/HistoryAtState/hsg-project/wiki/reviewing-frus-tei) for any clarification. 
4. When you have finished, commit the new file to GitHub and upload to hsg.  

---

**Do you have the XML for the volume?**

- [ ] If YES, proceed to **[step 4](#4-generate-tiffs-pngs-and-ebooks)**.

- [ ] If NO, skip step 4 and proceed to **[step 5](#5-update-volume-cache)**.

---

## 4. Generate TIFFs, PNGs, and eBooks

NOTE: In order to make TIFFs and PNGs, you need to already have Homebrew, Ghostscript, and Imagemagick installed. If you have already installed these dependencies, skip the steps below. 

### 4a. INSTALL HOMEBREW, GHOSTSCRIPT, IMAGEMAGICK

1. Follow the instructions at <https://github.com/HistoryAtState/hsg-project/wiki/Setup#installing-dependencies>, specifically, numbers 3 and 4.
2. In Terminal, enter `brew install imagemagick --with-libtiff` to install Imagemagick
3. Then, paste `brew install gs` to install Ghostscript. 

### 4b. CREATE TIFFS AND PNGS

If you have the final pdf of the volume, you can make TIFFs and PNGs while you wait for the final XML. To do this you will need to:

1. Create a new folder on your desktop, say, `frus-temp`, and place the final PDF in the folder, ensuring that you have renamed it according to our FRUS file naming conventions to match the volume's ID. 
2. Go to Spotlight in the upper-right hand corner of your Mac and search for “Terminal”. Open Terminal. 
3. Update Homebrew by entering `brew update && brew upgrade`
4. Type `cd ` (followed by a space), then drag and drop the folder you created in step 1 onto the Terminal window, which fills in the complete path to this directory. (The result will be a command in your Terminal that looks something like `cd /Users/joe/Desktop/frus-temp`.) Then paste this entire line into Terminal (note: you'll probably need to scroll to get the entire line's worth of text):

        for file in *.pdf; do FOLDER=`basename -s .pdf $file`; mkdir $FOLDER; cd $FOLDER; mkdir tiff; mkdir medium; mkdir pdf; cd ..; mv $file "$FOLDER/pdf/"; done

    This will create a parent folder with 3 subfolders named “pdf” “tiff” and “medium”. Then paste this entire line in:

        for folder in $(find * -maxdepth 0 -type d ); do gs -dBATCH -dNOPAUSE -q -sDEVICE=tiffg4 -r600 "-sOutputFile=$folder/tiff/%04d.tif" "$folder/pdf/$folder.pdf"; done

    This splits the pdf into tiffs, placing the tiffs in the “tiff” folder.

        for folder in $(find * -maxdepth 0 -type d ); do mogrify -path "$folder/medium" -format png -density 72 "$folder/tiff/*.tif"; mogrify -resize 'x800' "$folder/medium/*.png"; done

    This transforms the tiffs into screen resolution pngs that are in the “medium” folder. 

This final step takes significantly longer to complete so be sure that the process is done running before you upload the page images to S3. When this is done running, upload the parent folder with the 3 subfolders to S3 in the directory `static.history.state.gov/frus` 

---

**Have you installed Calibre?**

- [ ] If YES, skip step 4c proceed to **[step 4d](#4d-create-ebooks)**.

- [ ] If NO, proceed to **[step 4c](#4c-install-calibre)**.

---

### 4c. INSTALL CALIBRE

* Download the Calibre application:
  - If you are working on a Mac, download via https://calibre-ebook.com/download_osx
  - If you are working on a Windows, download via https://calibre-ebook.com/download_windows
* Double-click on the .dmg file you downloaded to open a folder window.
* Drag the Calibre book icon to the Applications folder within the window.
  - This action should begin the copying of Calibre to your local computer's Applications.
  - Your local Applications folder should then open automatically.
* Double-click on the new Calibre icon in your local Applications folder to open the program for the first time.
* In the set-up screen:
  - Accept the defaults on the first screen. Click "Next >"
  - Keep the default "Generic" in Manufacturers and "Generic e-ink device" in Devices. Click "Next >"
  - Click "Finish" to complete setup.
  - The Calibre application should then start.

### 4d. CREATE EBOOKS

Once you have the final XML for the volume and have reviewed it thoroughly, you are ready to make eBooks.

* In order to create eBooks, you will need to first open the volume XML and bibliography XML files in oXygen. (These files should be found in `hsg-project/repos/frus/volumes` and `hsg-project/repos/frus/bibliography` respectively.) 
* Using the external tools menu in oXygenXML, upload both of these files to localhost so that the latest copy is available to eXist: `Upload current file to localhost`
* Navigate to the Release app at <http://localhost:8080/exist/apps/release/>
* In the Release app, click `eBook Batch Helper`.
  * Set the radio button to generate `Both` epub and mobi-bound epubs.
  * Click `Generate eBooks`. 
* In your Downloads folder, you will see a folder entitled “frus-ebooks-[date]” with subfolders “epub” “mobi” and “mobi-bound”. 
  * When you see that the eBook batch conversion has successfully completed at the bottom of the eBook Batch Helper, open the “epub” subfolder in your Downloads folder. 
* In the folder, you should see a .epub file named with the volume’s frus-id. Open this file and review it in iBooks.
  * Verify the file is properly formatted:
    * Check the table of contents.
    * Click the hyperlinks for random documents.
    * Check headings
    * Check lists
    * Check tables
    * etc.
* If the .epud file is appropriately formatted, upload this file to:
  * the volume’s folder in S3, and 
  * in a new subfolder entitled “ebook”
* After you have uploaded the .epub of the volume, open the “mobi-bound” subfolder in your Downloads. 
  * You will notice that there is another .epub in this folder that has the same name as the file you just uploaded to S3.
    * NOTE: It is very important that when you are converting this file to a .mobi using Calibre that you use the file in the “mobi-bound” subfolder. If you accidentally use the .epub file in the “epub” subfolder to generate the .mobi, you will notice that all lists in the volume are incorrectly formatted as bulleted lists in the .mobi and you will have to reconvert using the mobi-bound epub. 
* Open Calibre and drag and drop the mobi-bound epub file into the app. 
  * It should appear as item number 1 in the center console. 
* Right click on the volume title and click `Convert Books` -> `Convert Individually`.
* In the new dialog box that pops up, change `Output format` in the upper right-hand corner of the box from “EPUB” to “MOBI”. Click `OK`. 
* When Calibre has finished the conversion, you will find the .mobi in Calibre Library in a subfolder that has the volume’s full name. Rename the .mobi according to our file naming protocol and open it using Kindle for Mac. 
* Review the .mobi as you did the epub:
  * Verify the file is properly formatted:
    * Check the table of contents.
    * Click the hyperlinks for random documents.
    * Check headings
    * Check lists
    * Check tables
    * etc.
* When you have completed the review of the .mobi file, upload it to S3 in the same directory as the .epub you uploaded above. 

As with the .pdf, in order to ensure that a link to both eBooks appears on the volume landing page, you will need to update the volume’s cache. Proceed to **[step 5](#5-update-volume-cache)**.  

## 5. Update Volume Cache

We are still able to use S3 Cache Helper, but there are several steps that we have to do differently to update the cache. 

You can find S3 Cache helper in the Release app at <http://localhost:8080/exist/apps/release>. 

If the menu on this page says, "S3 app isn't configured yet," you will need to follow these steps: 

1. Open oXygen and navigate to `hsg-project/repos/s3/modules` and open the file `aws_config_tmpl.xqm`
2. Create a copy of this file in the same directory by selecting `File > Save As`, and entering `aws_config.xqm` as the filename.
3. Enter the Access Key and Secret Key between the single quotes (`''`) in the file, and save the file.
4. Upload the file to localhost. 
5. Go back to the Release app and click Refresh. The message about the S3 app not being configured should be gone. You can now enter the S3 Cache Helper. 

Once in S3 Cache Helper, enter your volume’s ID and select `Submit`. (Whereas before, this automatically updated the cache for us, there are a few extra steps that we have to do in the new workflow.)

* After you press `Submit`, open your localhost in Transmit. (If you haven't configured Transmit, follow the setup directions at <https://github.com/HistoryAtState/hsg-project/wiki/Setup#connecting-to-hsg-with-transmit>.) Navigate down to `/db/apps/s3/cache/static.history.state.gov/frus`. Under `frus`, you should have a folder with the name of the volume you just submitted. Inside each of the subfolders, you should have a generated resources file. 

* To update the cache, leave your localhost open on Transmit and on the other side of the app open `1861.history.state.gov` and navigate to `apps/s3/cache/static.history.state.gov/frus`. 

* Move the volume folder from the localhost into the corresponding folder in `1861.history.state.gov`.

## 6. Add Subject Tags for the Volume

For each volume, the compiler is required to supply subject tags that must be entered and uploaded to hsg for every newly published volume. When you have received the tags from the compiler, you should use the Tag Helper to put them in proper form:

* Navigate to https://history.state.gov/cms/apps/tags/tag-helper.xq and enter the username and password you received during your training. 

* In the space provided, copy and paste the subject tags and press `Search`

Below the entry box, you should see a report that indicates how many tags were found for the number of tags you provided and a list of space-delimited tags for inserting into a TEI document. Double check that all of your tags have been found. If they haven't, double check the spelling, etc. against the list of approved tags and run the search again after correcting any errors. When all of your tags have been found, open the `frus-tags.xml` file at `/repos/tags/tagged-resources/frus`. In the file:

* Scroll down to the bottom of the document and locate the place where your volume should be inserted in the list of volumes that are already published. 

* Copy a `<study>` for another volume and paste it where your volume belongs.

* Revise the title and link information in this study to match your volume, and replace the existing `tag/@id` with the space-delimited tags you generated using Tag Helper above. 

When you have fully revised the new `<study>` entry for your volume in the file, save your changes and press `upload-current-file-to-history.state.gov` under your External Tools in oXygen.  

## 7. Create Carousel Entry

As with our previous workflow, in order to update the carousel for the publication of a new volume or quarterly release, you will need to create a new entry and update the display order. The files you need are now located at `hsg-project/carousel/display-order` and `hsg-project/carousel/entries`.

1. Open `display-order.xml` and add your new `<topic-id>` at the top of `display-order`, deleting the lowest `<topic-id>` entry so there are no more than 3 of these in `<display-order>`.
2. As with the shell files, to create a new topic entry, you can either open the most recent topics entry, edit the information for your volume—remembering to change the id to one higher, and save the file as a new entry, or your can open a blank XML document and paste in the following to edit and save in the repo:

`<?xml version="1.0" encoding="UTF-8"?>
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
</topic>`

Once you have finished editing both files, upload them to localhost, and review them one last time before uploading to hsg when you are are ready to publish the volume. 

## 7. Commit all files to GitHub
After you have finished publishing your volume, the files in each of these repos need to be committed to GitHub. Remember to click `Sync` after you commit the files to push your changes to the master branch. You will have to do this 3 separate times for the following files in 3 repos:
1.  carousel
    * a. display-order
    * b. numbered entry for volume
2. frus
    * a. bibliography
    * b. volumes
3. tags/tagged resources
    * a. frus-tags