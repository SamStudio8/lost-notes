## LFS
#### Pull all files at the same time

    git lfs fetch --all

## Misc

#### Executables

    git config -l | grep core.filemode
    git ls-files -s | grep my_bin.py
    git update-index --chmod=+x bin/my_bin.py

#### List blobs

     git rev-list --objects --all | git cat-file --batch-check='%(objectname) %(objecttype) %(rest)' | grep '^[^ ]* blob' | cut -d" " -f1,3-

https://stackoverflow.com/a/25954360
