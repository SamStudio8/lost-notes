## LFS
#### Pull all files at the same time

    git lfs fetch --all

## Misc

#### Check and fix executables

    git ls-files -s | grep my_bin.py
    git update-index --chmod=+x bin/my_bin.py
