# Command-line Notes

Find which process is listening on a given port:

    [sudo] lsof -i :6600

Recursive find-and-replace all instances of a string in a directory:

    find <mydir> -type f -exec sed -i 's/<string1>/<string2>/g' {} +

Inline search and replace:

    echo "${WORD/SEARCH/REPLACE}"

Transfer files from a remote host to a local machine with progress and compression:

    rsync -azP ricardo@192.168.1.81:/home/ricardo/Software/miniconda /home/ravila/Software/

Convert markdown to pdf:

    pandoc -f markdown Exam_3_cheatsheet.md -o Exam_3_cheatsheet.pdf

Grep PDF files:

    find /path -iname '*.pdf' -exec pdfgrep pattern {} +

