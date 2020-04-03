# Useful BASH One-liners

- [Useful BASH One-liners](#useful-bash-one-liners)
  - [Networking](#networking)
  - [File Transfers](#file-transfers)
  - [System](#system)
  - [Searching / Replacing](#searching--replacing)
  - [Images](#images)
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

Find your private IP address:

```
python -c "import socket; print(socket.gethostbyname(socket.gethostname()))"
```

Find your public IP address:

    curl ifconfig.io

## Searching / Replacing

Recursive find-and-replace all instances of a string in a directory:

    find <mydir> -type f -exec sed -i 's/<string1>/<string2>/g' {} +

Inline search and replace:

    echo "${WORD/SEARCH/REPLACE}"

Grep PDF files:

    find /path -iname '*.pdf' -exec pdfgrep pattern {} +

## Images

Convert format for multiple images:

```bash
for file in *.jpg; do convert $file ${file/.jpg/.png}; done
```
Resize multiple images:

```bash
for file in *.jpg; do convert -quality  90 -resize 1000x1000 $file $file; done
```
OCR an image:

    tesseract my-image output.txt 

## More PDF Stuff

Convert markdown to pdf:

    pandoc -f my-notes.md -o my-notes.pdf

OCR:

    ocrmypdf original-document.pdf new-document.pdf