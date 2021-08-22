## Miscellaneous commands ##

Unzip 7z files: `7z x <FILE>`

Unzip gzip files: `gzip -d <FILE>`

Get hashes
* MD5: `md5sum <FILE>`
* SHA1: `sha1sum <FILE>`
* SHA256: `sha256sum <FILE>`
* SHA512: `sha512sum <FILE>`

Get Human readable date from UNIX representation :
```
date -d @`printf "%d" <UNIX_DATE>`
```

Mount a volume with read only mode enable:
```
sudo mount -o ro <VOULUME> <PATH_TO_MOUNT>
```

Mount a volume with read only mode enable and offset if the data doesn't start in the first byte (each sector = 512 bytes): 
```
sudo mount -o ro,offser=<BYTES_VALUE_TO_MOUNT> <VOULUME> <PATH_TO_MOUNT>
```

## File System Operations ##

It is recommended to create several mounting directories ie. /mnt/loop0 /mnt/loop1 ...

Speak to the File System (FS) of the immage (if applicable): `debugfs <image>`

Commands in debugfs:
* can run: `ls -l`
* Check if there is a deleted file(s): `ll -ld`
* Get info of deleted file: `lsdel`
* Get a summary of the FS: `stats`
* Get info of specific inode:
	```
	stat <INODE_NUMBER> (include the <> symbols)
	````
* Get the contents of all the deleted files: `dump_unused`

Get info of FS: `fsstats <FS_IMAGE>`

Get info of all files (existing and deleted): `fls -r <FS_IMAGE>`

Get content of deleted file:
```
icat -r <FS_IMAGE> <INODE>
```

Get properties form a file on the FS_IMAGE:
```
istat <FS_IMAGE> <INODE>
```

Search for an inode at a block:
```
ifind -d <BLOCK_NUM> <FS_IMAGE>
```

Check if a block is part of a file (looking at the hex representation):
```
blkcat <FS_IMAGE> <BLOCK_NUM> | xxd | more
```

## Veracrypt ##

Decrypt encrypted files or FS. If we don't have any aditional info, we can press ENTER to skip:
```
veracrypt --text --truecrypt --mount <IMAGE_TO_MOUNT> <PATH_TO_MOUNT>
```

Unmount encrypted files or FS:
```
veracrypt --text --truecrypt --dismount  <PATH>
```

To mount a hidden volume, we need to only write the password of that hidden volume when mounting (if the volume that contains the hidden volume is already mounted, we need to umount it first):
```
veracrypt --text --truecrypt --mount <IMAGE_TO_MOUNT> <PATH_TO_MOUNT>
```

## Steganography ##

Check for steganography:
```
steghide info <FILE_NAME>
```

Extract files from a stegnographied file:
```
steghide extract -sf <FILE_NAME>
```

Get metadata:
```
exiftool <FILE_NAME>
```
## PDF Analysis ##

Get internal characteristics:
```
pdfid.py <FILE_NAME>
```

If we suspect that a PDF is used as a USB FS to store files:
```
mount_pdf <PDF_FILE_NAME> <DEST_TO_MOUNT> (optional & to send to bg)
```

Get location of JS code:
```
pdf-parser.py --raw --search javascript <PDF_FILE> 
```

Get info from a specific object:
```
pdf-parser.py -o <BLOCK_NUM> <PDF_FILE>
```

Get info from who calls an especific object:
```
pdf-parser.py -r <BLOCK_NUM> <PDF_FILE>
```

Get data of a certain object (could be a JS code):
```
python2 /usr/local/bin/pdf-parser.py -o <OBJECT_NUMBER> -f -w <PDF_FILE>
```
