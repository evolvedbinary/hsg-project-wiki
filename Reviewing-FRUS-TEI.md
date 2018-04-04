# Introduction

Reviewing the TEI edition of a FRUS volume (typically delivered by our vendors) is an essential step on the road to publishing the volume and adding it to the FRUS digital archive. The purpose of the review is to ensure that the volume meets our high standards for quality and to provide timely feedback to our vendors.  

This page contains the essential steps of a review.  As the page is expanded, links will be added to pages with more information.  

# Requirements

1. A history.state.gov development system on localhost (see [Setting up a history.state.gov development system](setup) for more information)
    1. oXygen XML Editor and GitHub Desktop
    1. An up-to-date checkout of the SVN repository
    1. A freshly repopulated database
1. A web browser
1. An ebook reader
1. The source volume (PDF or print)

# Review procedure

Before you begin, create a new [issue](https://github.com/HistoryAtState/frus/issues) for the volume, and paste our review checkbox template into the issue:

```
- [ ] Passes FRUS TEI Schematron check
- [ ] Passes RelaxNG schema check
- [ ] Scripts run
- [ ] Upload the FRUS TEI file into localhost
- [ ] Upload page images to S3
- [ ] Typography and characters
- [ ] Source note coding
- [ ] Table of contents
- [ ] Tables, both participant and other
- [ ] Openers and assorted segs
- [ ] Datelines added and prepped for dates project
- [ ] Postscripts and frus:attachments confirmed
- [ ] Front matter
- [ ] Back matter
- [ ] Random sample
- [ ] Volume metadata
- [ ] Ebook generation and review
```

As you complete the steps below, check off the boxes, and add any notes in comments. You can even ask other people on the team to weigh in in your comments, by referencing their GitHub username, e.g., [@joewiz](http://github.com/joewiz).

1. Prepare FRUS TEI file for review
    1. If the FRUS TEI file isn't already in GitHub, copy the FRUS TEI file into your GitHub working copy directory (`hsg-project/repos/frus/volumes`). The volume has to be in this directory in order for its references to the schema files to work properly.
    1. Open the file in oXygen
    1. Validate the document against our Schematron and RelaxNG schema using `Document` > `Validate` > `Validate`.
    1. Review the errors raised by the schema validation check. Errors we expect to fail include: 
        1. **`<editor>`** The names of the volume's compiler(s), as recorded in the `teiHeader` element. Check the volume's title page to see if the names are listed here; if so, give each name an `<editor>` element with a `@role` value matching their role as compiler (`primary`) or general editor (`general`) . If the names do not appear on the title page, check the volume's Preface, and identify the names of those noted specifically for having *compiled documentation* for the volume. Place them as `<editor role="primary"/>`. Copy these entries in the volume's metadata file in the `/db/cms/apps/volumes/data` folder, and in this file only, identify, if possible, the name of the person whose role most closely approximated the general editor; use the @role values and definitions in the volumes `code-tables/editor-role-codes.xml` file. 
        1. **`<graphic> @url`** - in a graphic/@url value of of `figure_nnnn`, `nnnn` coresponds to the page image filename, e.g., `nnnn.tif`. Download the volume's page images from the `paho-hcl` bucket's `UWDCC Scans` folder on S3, and open the image in Pixelmator. Check that there is only one image on this page. For each image on the page, perform the following:
            - Crop the image 
            - Use `File > Export` to save the cropped image as a compressed tif file, e.g., `figure_nnnn.tif` (not `.tiff`)
            - Use `Image > Image Size` to reduce the image size to max 500 pixels wide and 72 dpi, and use `File > Export` to save the resized image using the same `nnnn` number but prefixed with `figure_` and using the png format: `figure_nnnn.png`
            - Upload the newly generated .tif and .png files to S3's `static.history.state.gov` bucket in the `frus/{volume-id}` folder.  
            - Returning to the TEI, make sure the heading of the image is in the graphic element's `<head>` element - typing it in if not present. Ideally we should have a `<head>` element for each graphic; this is especially important for accessibility reasons, but helps everyone.
            - Revalidate the file
    1. Once the volume passes the schema and metadata checks, perform a `Format and Indent` (in the `Document` > `Source` menu) once and commit the file to the SVN repository
    1. Upload the FRUS TEI file into localhost
    1. Upload the volume's page images to S3 (or request help with this step)

1. Check typography and characters
    1. Confirm that no invalid characters have been used in the file. These are typically caused by poor proofing of OCR text. In oXygen, perform a search with the following parameters: 
        - Find: `[^a-zA-Z0-9\s!@#$%^&*()\[\]{}?+⅛⅘,.:;'"‘’“”°_\-–—/áäàâãćčéèûëíóòôöêšÉÖüñ|║ý‡<>=†¶§çỳž¢ÇŜ½⅓¼⅔¾£]`
        - These options checked: `Direction` > `Forward`, `Scope` > `All`, `Options` > `Regular expression` and `Wrap around`
       Select `Find All` and review the results. Just because a character is found doesn't mean it's wrong, but it can help flag problems like Cyrillic characters used in place of Roman characters, etc.
    1. Confirm that dashes and hyphens are used correctly:
        - For numeric ranges that should use en dashes (–), perform a search with the options `Options` > `Regular expression` and `Enable XML search options` > `Element contents` checked, for this regular expression: `\d[^–]\d`
        - Search for em dashes in element contents: `—`
        - Search for hyphens in element contents: `-` 
    1. Confirm that no straight single or double quotes are used, since all quotes should be curly/typographic:
        - Search for straight quotes in element contents with this regular expression: `['"]`
    1. Confirm there are no characters encoded using escaped entities instead of the actual character, e.g., `&#x2013;` instead of `—`. To search for any cases of this, search the document for `&#`.
    1. If you find systematic errors in any of these areas, return the volume to the vendor for correction.

1. Check the volume structure
    1. In your browser, open the volume's landing page: <http://localhost:8080/exist/apps/hsg-shell/historicaldocuments/{frus-volume-id}>
    1. In oXygen, expand the Outline to show the contents of the `front`, `body`, and `back`
    1. Verify the table of contents entries match the source volume
    1. Verify that the hierarchy is correct
    1. Verify that each entry is rendered in correct Sentence Case

1. Front and back matter
    1. Give each section of the front matter a general check, with the following special checks:
    1. Preface: Verify the styling of the closing signature
    1. Persons and Terms lists:
        1. Verify that entries are bold and definitions are not bold
        1. Verify that cross references ("See ___") are tagged with `<persName corresp="#{p_ID}" type="alt">` elements pointing to the alternate entry.
    1. Sources list: Verify the correct nesting of unpublished sources
    1. Index: 
        1. Verify the correct nesting of the entries
        1. Verify that the cross references are correctly tagged as document numbers vs. page numbers
        1. Verify that all cross references are tagged (and other numbers are not, e.g., years)

1. Body
    1. The volume should be delivered to us with extremely high accuracy.  To check this, we perform a thorough check on a random sample of 5% of the volume.  If the sample does not meet our standards, we will return the volume to the vendor for processing.
    1. To select the 5% of the volume, take the number of pages or documents and calculate the appropriate sample size: a 1000 page volume requires a sample of 50 pages, or a 300 document volume requires a sample of 15 documents.  
    1. Make a note of what sample you select, and include this information in your commit message following the review.  
    1. In your browser, turn on the "View Annotations mode" by appending `?view=annotations` to the URL. This mode reveals highlights instances of `<persName>`, `<gloss>`, and `<pb>` tags and shows what each link points to.  
    1. Perform the following checks on the sample:
    1. Verify that the people or terms have been tagged correctly. Only people and terms in the lists of persons and terms & abbreviations should be tagged, and to prevent misidentification, they should only be tagged when they appear in their full form. A "no ID" warning in the "view annotations mode" indicates that there is no `@target` or `@corresp` value; this is acceptable in document heading, where the sender and recipient are supposed to be tagged regardless of their but elsewhere all tagged text should have an ID.
    1. Similarly, verify that the entries in the right sidebar appear appropriately.
    1. Verify that the page breaks are correctly placed where the page in question begins; the `@n` values should match the source page numbers. Click on the page break entry to verify the the correct image is linked; `@facs` value should match the filename of the corresponding page image.
    1. Verify that all cross references in editorial notes and footnotes have been tagged and match our cross reference style guide. Verify correct interpretation of infra, supra, and ibid.
    1. Verify that all charts and tables are rendered correctly. Some tables are difficult to encode as TEI, and some issues can arise in the transformation from TEI into HTML.  
    1. Look for incorrect capitalization in the classification markings, dateline, and section headings. Title Case is preferable to ALL CAPS.
    1. Regardless of the styling in the printed book, Subject and Participant lists should be tagged as a `<list>` element with `<item>` children in the TEI. The list heading should be in Title Case, and the entries will begin on the line following the list heading. 
    1. Paragraphs: The actual text within each document is either coded as a paragraph `<p>` or as a `<list>`. The `<list>` element is used to indicate a series of tightly spaced lines (often used in quotes or lists), but these instances need not actually contain bulleted or numbered items.  The tight spacing helps group lines together.

1. Check the volume metadata
    1. Open the volume's metadata file in `hsg-project/repos/frus/bibliography/` alongside the TEI file's `teiHeader` and `titlePage`
    1. Verify that the title, editors, and publication information is correct
        1. Verify that year ranges use en-dashes (–) rather than hyphens (-). 
    1. For volumes published since 1972, the volume's ISBN should be present. If needed, look up the ISBN on WorldCat or Amazon and add it to both the volume and its metadata file. If it can't be found, add an XML comment: `<!-- ISBN not found on WorldCat or Amazon -->`
    1. The title elements should all be filled in. The `@type="subseries"` should only contain the date range.

1. Other XML checks
    1. The table of contents (@xml:id="toc") should be a nested list and should match the source table of contents, with page numbers tagged as page number cross references, e.g., `<ref target="#pg_3">3</ref>`
    2. In some volumes, one entry may refer to two different terms, e.g., NAT(O), refers both to the North Atlantic Treaty Organization and the North Atlantic Treaty. As a result, make sure both NATO and NAT are tagged correctly throughout the text. There is a large possibility that they will  not be tagged if the vendor simply searched for NAT(O). 
    1. Check that all source notes have the `note/@type="source"` attribute (all documents except editorial notes) should have these. XPath: `//tei:div/tei:note[@type='source']`

1. Ebook
    1. See the wiki article on generating ebooks, <https://github.com/HistoryAtState/hsg-project/wiki/ebooks>.

Finally, if time allows, flip through the volume (this is fastest with the ebook).  Let your eye spot any anomalies.