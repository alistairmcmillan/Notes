# Rsync Notes

## Cloning with Rsync

Cloning the content of an NTFS drive to a FAT32 drive.

    rsync -avz --progress --modify-windows=2 --delete --no-p --dry-run /media/sda2/Users/John /media/sdb1/John

    -a same as rlptgoD
        -r recursive
        -l copy links
        -p preserve perms
        -t preserve times
        -g preserve groups
        -o preserve owner
        -D preserve devices and preserve specials
    -v verbose
    -z compress
    --delete to remove files that exist in the destination but not the source
    --dry-run
    --modify-windows=2 because NTFS and FAT32 stores timestamps differently
    --no-p so that persmissins aren't copied
