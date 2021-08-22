## Miscellaneous commands ##

Unzip 7z files: `7z x <FILE>`

Unzip gzip files: `gzip -d <FILE>`

Get hashes
* MD5: `md5sum <FILE>`
* SHA1: `sha1sum <FILE>`
* SHA256: `sha256sum <FILE>`
* SHA512: `sha512sum <FILE>`

Get Human readable date from UNIX representation 
```
date -d @`printf "%d" <UNIX_DATE>`
```

Mount a volume with read only mode enable 
```
sudo mount -o ro <VOULUME> <PATH_TO_MOUNT>
```

Mount a volume with read only mode enable and offset if the data doesn't start in the first byte (each sector = 512 bytes): 
```
sudo mount -o ro,offser=<BYTES_VALUE_TO_MOUNT> <VOULUME> <PATH_TO_MOUNT>
```
