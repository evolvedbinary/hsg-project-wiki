## Overview of procedures for digitizing a FRUS volume

1. Prepare source images for vendor
  1. Obtain source images of printed book
  1. Upload images to vendor directory on S3
1. Prepare TEI shell files for vendor
  1. Create TEI shell files for the volumes
  1. Send TEI shells to vendor
1. Receive and review delivery from vendor
  1. Receive delivery of completed TEI files from vendor
  1. Review delivery, and return for revision or accept
1. Prepare final images
  1. Crop graphics
  1. Convert graphics to PNG
  1. Convert page images to PNG
  1. Upload all to public S3 folder

## Prepare images

Our archival scans of FRUS are already all uploaded to the `paho-hcl` bucket, inside the `UWDCC Scans` folder.

## Prepare TEI shell files

A shell file contains the basic TEI Header that our vendor will add the contents of the volume into. We generate the shell file from two sources: 

1. Our `volumes` app data (`/db/cms/apps/volumes/data`)
2. The TEI.2 and entity DTD files that UWDCC provided

The first step is to review our `volumes` app data for the volume(s) in question. Make sure that all of the `<title>` elements are filled in.

The second step is to download the TEI.2 and DTD files. Download the `.ent` and `.xml` files for the volume(s) in question from the `paho-hcl` bucket, inside the `UWDCC TEI` folder. Open the .ent file and make sure that each image's ID corresponds to the filename; if there is a mismatch, make a note of which images are affected, because we'll need to fix the `<pb>` tags in the final shell file.

Next, upload the TEI.2 files (the `.xml` files) to `/db/apps/uwdcc/tei2`. If needed, also create `/db/apps/uwdcc/new`.  Then run this XQuery script in eXide, being sure to first log in as a dba user:

```xquery
xquery version "3.0";

declare function local:uwdcc-to-ho($nodes) {
    for $node in $nodes
    return
        typeswitch ($node)
            case element(TEI.2) return local:TEI.2($node)
            case element(teiHeader) return local:teiHeader($node)
            case element(text) return element text { local:recurse($node) }
            case element(front) return element front { local:recurse($node) }
            case element(body) return element body { local:recurse($node) }
            case element(back) return element back { local:recurse($node) }
            case element(div1) return local:div($node)
            case element(head) return $node
            case element(pb) return local:pb($node)
            default return local:recurse($node)
};

declare function local:recurse($node) {
    for $child in $node/node()
    return
        local:uwdcc-to-ho($child)
};

declare function local:TEI.2($node) {
    let $vol := collection('/db/cms/apps/volumes/data')//location[@loc='madison'][ends-with(., substring-before(util:document-name($node), '.xml'))]/ancestor::volume
    let $vol-id := $vol/@id
    return
        element TEI { 
            attribute xml:id {$vol-id}, 
            local:recurse($node) 
        }
};

declare function local:teiHeader($node) {
    let $vol := collection('/db/cms/apps/volumes/data')//location[@loc='madison'][ends-with(., substring-before(util:document-name($node), '.xml'))]/ancestor::volume
    return
        <teiHeader>
            <fileDesc>
                <titleStmt>
                    <title type="complete">{$vol/title[@type='complete']/string()}</title>
                    <title type="series">{$vol/title[@type='series']/string()}</title>
                    <title type="subseries">{$vol/title[@type='sub-series']/string()}</title>
                    <title type="volumenumber">{$vol/title[@type='volumenumber']/string()}</title>
                    <title type="volume">{$vol/title[@type='volume']/string()}</title>
                    {$vol/editor}
                </titleStmt>
                <publicationStmt>
                    <publisher>United States Government Printing Office</publisher>
                    <pubPlace>Washington</pubPlace>
                    <date>{$vol/published-year/string()}</date>
                    <idno type="dospubno"></idno>
                    <idno type="isbn-10">{$vol/isbn10/string()}</idno>
                    <idno type="isbn-13">{$vol/isbn13/string()}</idno>
                    <idno type="frus">{$vol/@id/string()}</idno>
                    <idno type="oclc">{$vol/location[@loc='worldcat']/string()}</idno>
                </publicationStmt>
                <sourceDesc><p>Released in {$vol/published-year/string()} as printed book and adapted to TEI-XML based on scans provided by UWDCC</p></sourceDesc>
            </fileDesc>
            <encodingDesc>
                <editorialDecl>
                    <correction>
                        <p>The following errors in the original edition have been corrected: <list>
                            <item></item>
                        </list></p>
                        <p>The following issues are known and can't be fixed: <list>
                            <item></item>
                        </list>
                        </p>
                    </correction>
                </editorialDecl>
            </encodingDesc>
            <revisionDesc>
                <change>{adjust-date-to-timezone(current-date(), ())} Joe Wicentowski (PA/HO): Created TEI shell for HCL based on UWDCC submission</change>
            </revisionDesc>
        </teiHeader>
};

declare function local:div($node) {
    let $type := lower-case(replace($node/@type, '\s+', '-'))
    return 
        element div { attribute type {$type}, local:recurse($node) }
};

declare function local:pb($node) {
    let $n := $node/@n
    let $xmlid-base := if (contains($n, '[')) then substring-before(substring-after($n, '['), ']') else $n
    let $xmlid := if ($xmlid-base ne '') then concat('pg_', $xmlid-base) else concat('pg-seq-', local:index-of(root($node)//pb, $node))
    let $facs := substring-after($node/following-sibling::element()[1]/@entity, '.p')
    return
        element pb { 
            attribute n {$n}, 
            attribute xml:id {$xmlid},
            attribute facs {$facs}
        }
};

declare function local:index-of($seq as node()*, $n as node()) as xs:integer* {
    local:index-of($seq, $n, 1)
};

declare function local:index-of($seq as node()*, $n as node(), $i as xs:integer) as xs:integer* {
    if ( empty($seq) ) then
        ()
    else if ( $seq[1] is $n ) then
        ( $i, local:index-of(remove($seq, 1), $n, $i + 1) )
    else
        local:index-of(remove($seq, 1), $n, $i + 1)
};

declare function local:normalize-ns($nodes as node()*) {
    for $node in $nodes
    return
        typeswitch($node)
            case element() return
                element { QName("http://www.tei-c.org/ns/1.0", local-name($node)) } {
                    $node/@*,
                    local:normalize-ns($node/node())
                }
            default return
                $node
};


let $uwdcc-col := '/db/apps/uwdcc/tei2'
let $new-col := '/db/apps/uwdcc/new'
let $docs := 
    collection($uwdcc-col)
    (: 
    let $vol-ids := 'frus1949v05'
    for $vol-id in $vol-ids 
    return
        doc(concat($uwdcc-col, '/', $vol-id, '.xml'))
    :)
for $doc in $docs
let $vol-id := collection('/db/cms/apps/volumes/data')//location[@loc='madison'][ends-with(., $doc/TEI.2/@id)]/ancestor::volume/@id
let $xformed := local:normalize-ns(local:uwdcc-to-ho($doc))
order by $vol-id
return
    xmldb:store($new-col, concat($vol-id, '.xml'), $xformed)
```

This script merges the TEI.2 and our volumes data into a new TEI shell, stored in `/db/apps/uwdcc/new`.  Download the resulting files, and open them, ensuring that titles, editors, etc. are in order.

Then send the TEI shell files to the vendor.  

## Receive delivery

Once the vendor finishes preparing the TEI version of a volume, they will email the delivery. Download the delivery to your computer, and copy the file(s) into the `/db/cms/apps/tei-content/frus-volumes` folder, so that when you open the file in oXygen, the relative path to the schematron files will be correct. Open each file in oXygen, and run a schema check.

Review the volume as described in [Reviewing FRUS TEI](reviewing-frus-tei), fixing or noting issues that the vendor needs to fix.  

## Prepare graphics and page images

### Install ImageMagick for converting TIFF to PNG

1. Open Terminal.app
1. Install homebrew via http://brew.sh
1. Install ImageMagick with `brew install imagemagick --with-libtiff`
1. Install GhostScript with `brew install gs`

### Crop images

If any of the schematron errors relate to images that can't be found on S3, you will need to crop these images from our original TIFFs.

An XPath command for locating the images in a TEI file is: `//graphic`. Here's an example `<graphic>` element:

    <figure><graphic url="figure_0637"/></figure>

The schematron file will look for two files on S3: `figure_0637.tif` and `figure_0637.png`. The `0637` portion of the filename refers to `<pb facs="0637">`, which corresponds to a TIFF file, `0637.tif`, in the original directory of TIFF files (`paho-hcl/UWDCC Scans`).  Find this TIFF file and crop the image into a new file, `figure_0637.tif`.

- Ensure all TIFF files end with `.tif` instead of `.tiff`. To convert `*.tiff` to `*.tif`, use this command: 

```
for FILE in $(find * -name '*.tiff' -type f); do BASE=`basename $FILE .tiff`; mv $FILE $BASE.tif; done;
```

- Convert big TIFFs to screen res PNGs:

```bash
mogrify -format png -density 72 "*.tif"; mogrify -resize '600x800' "*.png"
```

Now upload each volumes' images into the `static.history.state.gov` bucket in the `frus` directory, e.g., `frus/frus1969-76v01/figure_0637.tif` and `frus/frus1969-76v01/figure_0637.png`. 

## Convert all page image TIFFs to PNGs

We also prepare page images as screen resolution PNGs. Check the `static.history.state.gov` bucket in the `frus` directory to see if the TIFF and PNG files are already in the volume's `tiff` and `medium` folders, respectively. If the TIFF and PNG files haven't been uploaded, here's how:

- Download the TIFFs for all volumes, e.g., to `~/Desktop/frus-images`, so that each volume's TIFFs are inside a subfolder like `frus-images/frus1969-76v01`, `frus-images/frus1969-76v02`, etc.
- In Terminal.app, cd into the `frus-images` folder
- This command will create medium, tiff folders, move tiffs into tiff folder:

```bash
for FOLDER in $(find * -maxdepth 0 -type d ); do mkdir $FOLDER/tiff; mkdir $FOLDER/medium; mv $FOLDER/*.tif $FOLDER/tiff; done
```

- This command will convert and mogrify TIFFs into PNGs. It's slow but produces the best quality of any resizing utility we've found:

```bash
for folder in $(find * -maxdepth 0 -type d ); do mogrify -path "$folder/medium" -format png -density 72 "$folder/tiff/*.tif"; mogrify -resize 'x800' "$folder/medium/*.png"; done
```

Now upload each volumes' images into the `static.history.state.gov` bucket in the `frus` directory, e.g., `frus/frus1969-76v01/medium/0001.png` and `frus/frus1969-76v01/tiff/figure_0637.tif`. 