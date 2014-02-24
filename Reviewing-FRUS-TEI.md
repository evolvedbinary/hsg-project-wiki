# Introduction to reviewing FRUS TEI

Reviewing the TEI edition of a FRUS volume (typically delivered by our vendors) is an essential step on the road to publishing the volume and adding it to the FRUS digital archive. The purpose of the review is to ensure that the volume meets our high standards for quality and to provide timely feedback to our vendors.  

The following checklist captures the essential steps of a review.  Each step links to a page with more information.  

1. Requirements
    1. A history.state.gov development system on localhost
        1. oXygen XML Editor and Subversion (SVN) Client
        1. An up-to-date checkout of the SVN repository
        1. A freshly repopulated database
    1. A web browser
    1. An ebook reader
    1. The source volume (PDF or print)

1. Prepare FRUS TEI file for review
    1. Copy the FRUS TEI file into your SVN working copy directory (`paho-trunk/db/cms/apps/tei-content/data/frus-volumes`)
    1. Open the file in oXygen
    1. Apply the FRUS TEI Schematron and RelaxNG schema files to check for structural problems
    1. Open the volume's metadata file in `paho-trunk/db/cms/apps/volumes/data/` alongside the TEI file's `teiHeader` and `titlePage` and verify that the title, editors, and publication information is correct
    1. Once the volume passes the schema and metadata checks, commit the file to the SVN repository
    1. Upload the FRUS TEI file into localhost
    1. Upload the volume's page images to S3

1. Check the volume structure
    1. In your browser, open the volume's landing page: `http://localhost:8080/historicaldocuments/{frus-volume-id}`
    1. In oXygen, expand the Outline to show the contents of the `front`, `body`, and `back`
    1. Verify the table of contents entries and hierarchy match the source volume
    1. Correct case (all caps > sentence case)

1. Check page numbers and images
    1. Pick 3 pages from the front matter and 5 pages from the body
    1. Verify the `<pb>` elements: the `@n` values should match the source page numbers, and the `@facs` values should match the filename of the page images.