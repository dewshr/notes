# Useful BASH One-liners

- [Useful BASH One-liners](#useful-bash-one-liners)
  - [Networking](#networking)
  - [File Transfers](#file-transfers)
  - [System](#system)
  - [Searching / Replacing](#searching--replacing)
  - [More PDF Stuff](#more-pdf-stuff)

## Networking

Find which process is listening on a given port:

    [sudo] lsof -i :6600

## File Transfers

Transfer files from a remote host to a local machine with progress and compression:

    rsync -azP remote-user@remote-host:~/Remote/Path ~/Destination

## System

Find number of processors:

    nproc

Find your public IP address:

    curl ifconfig.io

## Searching / Replacing

Recursive find-and-replace all instances of a string in a directory:

    find <mydir> -type f -exec sed -i 's/<string1>/<string2>/g' {} +

Inline search and replace:

    echo "${WORD/SEARCH/REPLACE}"

Grep PDF files:

    find /path -iname '*.pdf' -exec pdfgrep pattern {} +

## More PDF Stuff

Convert markdown to pdf:

    pandoc -f my-notes.md -o my-notes.pdf

OCR:
```bash
# Convert to images and run
tesseract my-image output 

# ...or (using tesseract under the hood):
ocrmypdf original-document.pdf new-document.pdf
```