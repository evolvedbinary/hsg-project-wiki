# Ebooks

We publish EPUB and Mobi versions of each FRUS volume. This page explains how we generate and review ebooks before publication.

## Prerequisites

1. An [up-to-date](version-control) [history.state.gov development system](setup)
1. [Calibre](http://calibre-ebook.com/download)
1. [iBooks](https://www.apple.com/support/mac-apps/ibooks/) for Mac (installed by default with Mac OS X)
1. [Kindle](https://itunes.apple.com/us/app/kindle/id405399194?mt=12) for Mac (free from the Mac App Store)
1. Internet connection (to access cover art, graphics on S3)

## Preparation

1. Check that the cover images for the volumes are all in S3; check the `static.history.state.gov` bucket under `frus/{volume-id}/covers`. Without these covers, the ebooks you generate below have a generic cover image, which is okay for proofing but not for publication. 
1. Validate each volume's TEI file in oXygen against the `frus.sch` and `frus.rnc` schema files in oXygen. It's important to fix as many problems as possible before generating the ebook, since the ebook generating process takes time.
1. Similarly, ensure that the volume titles and other metadata in `/db/cms/apps/volumes/data` for each volume is complete.
1. If you want to quickly preview how a volume will look as an ebook, go to https://localhost:8443/cms/apps/epub and select a volume. This can be handy but isn't a substitute for generating the various formats of ebooks and reviewing them in a real ebook reader.

## Generating Ebooks

1. Open the Ebook Batch Helper at https://localhost:8443/cms/apps/tei-content/views/ebook-batch
1. Paste in the desired volume IDs, one per line
1. Select the desired output format(s): `EPUB`, `Mobi-bound EPUB`, or `Both`
1. Select the `Generate Ebooks` button. **Note**: The Ebook Batch Helper can take 5-10 minutes to generate each ebook; particularly slow are page-based-cross-reference-type volumes of the pre-Johnson era. Since the Ebook Batch Helper doesn't provide any feedback or progress indicators until it has completed all ebooks, the best way to monitor progress is by using the Monex Console: http://localhost:8080/apps/monex/console.html; the Console shows detailed progress messages. If you need to cancel the Ebook Batch Helper, use the Monex Monitoring page: http://localhost:8080/apps/monex/index.html, clicking the "X" beside the ebook-generate.xq query in the "Running Queries" window.
1. When the Ebook Batch Helper is done, it will produce a final report about the results of the conversion: errors are noted in red, successful conversions in green. Mobi conversion via Calibre is reported in its own window.
1. The Ebook Batch Helper saves ebooks to your home folder's `Downloads` folder inside a date-stamped folder called `frus-ebooks-yyyy-mm-dd`. Here, EPUB files are stored in the `epub` directory and Mobipocket files in the `mobi` directory. A third directory, `mobi-bound`, contains intermediate Mobi-bound EPUB-formatted files, which can be discarded once the final Mobi files have been generated. As the tool runs, you can open your `Downloads/frus-ebooks-yyyy-mm-dd` folder on your desktop, and watch as the contents are filled in: first, the `epub` and `mobi-bound` folders, then the `mobi` folders.
1. You can re-run the tool to generate a new set; new ebook files will overwrite the existing ones.

## Reviewing Ebooks

1. Drag a volume's EPUB file into oXygen (**not** the Mobi-bound EPUB or Mobi files). This opens a new Archive Browser pane, showing the contents of the EPUB file. Select the "Validate" button (red checkmark icon) in the Archive Browser pane. This runs the [EpubCheck](http://www.oxygenxml.com/xml_editor/epub.html) utility, which reports any structural problems with the ebook. Errors like `element "ul" not allowed here` can generally be ignored at present, but errors that we do try to fix include `missing`, `not found`, or `fragment identifier not defined` errors relating to links to pages and their fragments, and referenced resources like images. 
1. Open the EPUB files in iBooks and the Mobipocket files in Kindle. Scan the contents of the volume to make sure the volume looks alright, paying particular attention to any formatting that might be out of the ordinary.

## Uploading Ebooks for Publication

1. Upload each volume's `.epub` and `.mobi` file to S3, under frus/{volume-id}/ebooks. Use the AWS S3 console (web) or an S3 client like Transmit.
2. When ready for public release, use oXygen's Database Browser to edit history.state.gov's `/db/history/data/s3-resources` folder to add entries for both epub and mobi files. Without this, links to the volumes will not appear on the volumes' landing pages or the [listing of all ebooks](history.state.gov/historicaldocuments/ebooks).