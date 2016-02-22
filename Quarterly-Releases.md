# FRUS quarterly releases

(under construction)

## Timeline

- 4 weeks before HAC, establish volumes to be included in the release
- Run Quarterly Release Helper to generate draft tweets and press release at https://localhost:8443/cms/apps/tei-content/views/quarterly-release
- Circulate list of volumes to management for editing announcements
- Generate cover images for ebooks, upload to S3
- Post materials to S3, hsg, and GitHub
- Release announcements

## What is uploaded where

- Ebooks and covers are uploaded to S3: `frus/{vol-id}/ebooks`, `frus/{vol-id}/covers`
- Once ebooks have been posted, run the S3 Cache Helper at https://history.state.gov/cms/apps/aws/edit/update-leaf-directory
- Upload FRUS TEI and metadata to hsg: `/db/cms/apps/tei-content/data/frus-volumes` and `/db/cms/apps/volumes/data`
- Post FRUS TEI for new releases and any volumes or schemas updated since the last release to GitHub: `https://github.com/HistoryAtState/frus`

## Posting FRUS TEI to GitHub

- Besides uploading the newly released volumes, check for any volumes that've been updated since the last release.
- Tip: Use oXygen's Compare Directories tool (under the Tools menu) to compare `/db/cms/apps/tei-content/data/frus-volumes` to your checked out copy of the FRUS repo's `volumes` directory. Double check the list of volumes against those already published on hsg. Be sure not to include volumes that are merely shell files (as indicated by their small file size).