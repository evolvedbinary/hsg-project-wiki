# Introduction

Reviewing the TEI edition of a FRUS volume (typically delivered by our vendors) is an essential step on the road to publishing the volume and adding it to the FRUS digital archive. The purpose of the review is to ensure that the volume meets our high standards for quality and to provide timely feedback to our vendors.  

The following checklist captures the essential steps of a review.  Each step links to a page with more information.  

# Requirements

1. A history.state.gov development system on localhost
    1. oXygen XML Editor and Subversion (SVN) Client
    1. An up-to-date checkout of the SVN repository
    1. A freshly repopulated database
1. A web browser
1. An ebook reader
1. The source volume (PDF or print)

# Review procedure

1. Prepare FRUS TEI file for review
    1. Copy the FRUS TEI file into your SVN working copy directory (`paho-trunk/db/cms/apps/tei-content/data/frus-volumes`)
    1. Open the file in oXygen
    1. Apply the FRUS TEI Schematron and RelaxNG schema files to check for structural problems
    1. Open the volume's metadata file in `paho-trunk/db/cms/apps/volumes/data/` alongside the TEI file's `teiHeader` and `titlePage` and verify that the title, editors, and publication information is correct
    1. Once the volume passes the schema and metadata checks, commit the file to the SVN repository
    1. Upload the FRUS TEI file into localhost
    1. Upload the volume's page images to S3 (or request help with this step)

1. Check the volume structure
    1. In your browser, open the volume's landing page: `http://localhost:8080/historicaldocuments/{frus-volume-id}`
    1. In oXygen, expand the Outline to show the contents of the `front`, `body`, and `back`
    1. Verify the table of contents entries match the source volume
    1. Verify that the hierarchy is correct
    1. Verify that each entry is rendered in correct Sentence Case

1. Front and back matter
    1. Give each section of the front matter a general check, with the following special checks
    1. Preface: Verify the styling of the closing signature
    1. Persons and Terms lists:
        1. Verify that entries are bold and definitions are not bold
        1. Verify that cross references ("See ___") are tagged with `<ref>` elements pointing to the alternate entry.
    1. Sources list: Verify the correct nesting of unpublished sources
    1. Index: 
        1. Verify the correct nesting of the entries
        1. Verify that the cross references are correctly tagged as document numbers vs. page numbers
        1. Verify that all cross references are tagged (and other numbers are not, e.g., years)

1. Body
    1. The volume should be delivered to us with extremely high accuracy.  To check this, we perform a thorough check on a random sample of 5% of the volume.  If the sample does not meet our standards, we will return the volume to the vendor for processing.
    1. To select the 5% of the volume, take the number of pages or documents and calculate the appropriate sample size: a 1000 page volume requires a sample of 50 pages, or a 300 document volume requires a sample of 15 documents.  
    1. Make a note of what sample you select, and include this information in your commit message following the review.  
    1. Perform the following checks on the sample:
    1. In your browser, turn on the "View Annotations mode" by appending `?view=annotations` to the URL. This mode reveals highlights instances of `<persName>`, `<gloss>`, and `<pb>` tags and shows what each link points to.  
    1. Verify that the people or terms have been tagged correctly. Only people and terms in the lists of persons and terms & abbreviations should be tagged, and to prevent misidentification, they should only be tagged when they appear in their full form. A "no ID" warning in the "view annotations mode" indicates that there is no `@target` or `@corresp` value; this is acceptable in document heading, where the sender and recipient are supposed to be tagged regardless of their but elsewhere all tagged text should have an ID.
    1. Similarly, verify that the entries in the right sidebar appear appropriately.
    1. Verify that the page breaks are correctly placed where the page in question begins; the `@n` values should match the source page numbers. Click on the page break entry to verify the the correct image is linked; `@facs` value should match the filename of the corresponding page image.
    1. Verify that all cross references in editorial notes and footnotes have been tagged and match our cross reference style guide. Verify correct interpretation of infra, supra, and ibid.
    1. Verify that all charts and tables are rendered correctly. Some tables are difficult to encode as TEI, and some issues can arise in the transformation from TEI into HTML.  
    1. Look for incorrect capitalization in the classification markings, dateline, and section headings. Title Case is preferable to ALL CAPS.
    1. Regardless of the styling in the printed book, Subject and Participant lists should be tagged as a `<list>` element with `<item>` children in the TEI. The list heading should be in Title Case, and the entries will begin on the line following the list heading. 
    1. Paragraphs: The actual text within each document is either coded as a paragraph `<p>` or as a `<list>`. The `<list>` element is used to indicate a series of tightly spaced lines (often used in quotes or lists), but these instances need not actually contain bulleted or numbered items.  The tight spacing helps group lines together.

1. Ebook
    1. To generate the ebook, go to the TEI Content app (`http://localhost:8080/cms/apps/tei-content`), navigate to the volume, and select `Download EPUB`. 
    1. TODO: Add directions for generating the MobiPocket version.

Finally, if time allows, flip through the volume (this is fastest with the ebook).  Let your eye spot any anomalies.