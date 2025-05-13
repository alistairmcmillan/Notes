# Ghostscript Notes

## How to resave a PDF file with images compressed

    gs -sDEVICE=pdfwrite -dCompatibilityLevel=1.4 -dPDFSETTINGS=/ebook -dNOPAUSE -dQUIET -dBATCH -sOutputFile="output.pdf" "input.pdf"
    
    Change "ebook" to "printer" for better quality

    Source: https://www.reddit.com/r/opensource/comments/ou4y5h/is_there_an_open_source_program_to_reduce_the/h72nl4g/
