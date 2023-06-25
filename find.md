# find Notes

## Find files with a certain extension

    find . -type f -name "*.lnk" -print

## Find files over a certain size

    find . -type f -size +100000k -print

## Find empty directories

    find . -empty -type d -print

## Find empty files

    find . -empty -type f -print
