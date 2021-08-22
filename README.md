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
## Clamscan (Antivirus) ##
Run clamsan AV to a directory:
```
clamscan -ri <DIRECTORY_TO_SCAN>
```

## Windows Registry ##

Windows Registry: Contains all information about which programs are installed, users, logs, events, activation keys, 
* regedit

Initial regedit keys:
* HKEY_LOCAL_MACHINE or HKLM [IN C:\windows\system32\config]
	* HKEY_LOCAL_MACHINE\SAM		SAM
	* HKEY_LOCAL_MACHINE\Security	security
	* HKEY_LOCAL_MACHINE\Software	software
	* HKEY_LOCAL_MACHINE\Systemsystem
* HKEY_CLASSES_ROOT or HKCR
* HKEY_USERS or HKU
	* HK_USERS\DEFAULT	[in C:\windows\system32\config\default]
* HKYE_CURRENT_CONFIG or HKCC
* HKEY_CURRENT_USER or HKCU 	[home-user files NTUSER.DAT and USRCLASS.DAT]

Browsing history of IE is stored in:
*/Users/issa/AppData/Local/Historial/History.IE5/MSHist##################/index.dat*

Representation	|	## ######## ########
---------------	| -----------------------
Format	|	cc yyyymmdd yyyymmdd
Example|	01 20210808 20210800

Temp files from IE can be found in:
* Spanish: *'/Users/issa/AppData/Local/Archivos temporales de Internet/Low/Content.IE5/'*

#### Get Windows info ####

Get product name complete windows name:
```
reglookup -p /Microsoft/Windows\ NT/CurrentVersion/ProductName <MOUNTED_DIR>/Windows/System32/config/SOFTWARE
```
Or
```
reglookup -p '/Microsoft/Windows NT/CurrentVersion/ProductName' <MOUNTED_DIR>/Windows/System32/config/SOFTWARE
```

Get product id:
```
reglookup -p '/Microsoft/Windows NT/CurrentVersion/ProductID' <MOUNTED_DIR>/Windows/System32/config/SOFTWARE
```

Get build number or SP:
```
reglookup -p '/Microsoft/Windows NT/CurrentVersion/CurrentBuildNumber' <MOUNTED_DIR>/Windows/System32/config/SOFTWARE
```

Get registered owner of the Windows license:
```
reglookup -p '/Microsoft/Windows NT/CurrentVersion/RegisteredOwner' <MOUNTED_DIR>/Windows/System32/config/SOFTWARE
```

Get registered organization of the Windows license:
```
reglookup -p '/Microsoft/Windows NT/CurrentVersion/RegisteredOrganization' <MOUNTED_DIR>/Windows/System32/config/SOFTWARE
```

Get OS installation date (UNIX Time format):
```
reglookup -p '/Microsoft/Windows NT/CurrentVersion/InstallDate' <MOUNTED_DIR>/Windows/System32/config/SOFTWARE
```

Get computer name:
```
reglookup -p '/ControlSet002/Control/ComputerName/ComputerName' <MOUNTED_DIR>/Windows/System32/config/SYSTEM
```

Get TimeZone:
```
reglookup -p '/ControlSet002/Control/TimeZoneInformation/StandardName' <MOUNTED_DIR>/Windows/System32/config/SYSTEM
```
```
reglookup -p '/ControlSet002/Control/TimeZoneInformation/DaylightName' <MOUNTED_DIR>/Windows/System32/config/SYSTEM
```

Get all installed software:
```
reglookup -p '/Microsoft/Windows NT/CurrentVersion/Uninstall' <MOUNTED_DIR>/WINDOWS/System32/config/software
```

Get all system users:
```
samdump2 <MOUNTED_DIR>/WINDOWS/system32/config/system <MOUNTED_DIR>/WINDOWS/system32/config/SAM | cut -d':'-f1'
```

Get network interfaces configuration:
```
reglookup -p /ControlSet002/Services/Tcpiip/Parameters/Interfaces <MOUNTED_DIR>/WINDOWS/system32/config/system
```

Get browsing history of IE:
```
pasco <MOUNTED_DIR>/Users/<USERNAME>/AppData/Local/Historial/History.IE5/MSHIST<DATE_FORMAT>/index.dat
```

### Windows Firewall Configuration ###

Check if firewall is enabled:
```
reglookup -p /ControlSet002/Services/SharedAccess/Parameters/FirewallPolicy/StandardProfile/EnableFirewall <MOUNTED_DIR>/WINDOWS/system32/config/system
```

Check allowed exceptions:
```
reglookup -p /ControlSet002/Services/SharedAccess/Parameters/FirewallPolicy/StandardProfile/DoNotAllowExceptions <MOUNTED_DIR>/WINDOWS/system32/config/system
```

Check disabled notifications:
```
reglookup -p /ControlSet002/Services/SharedAccess/Parameters/FirewallPolicy/StandardProfile/DisableNotifications <MOUNTED_DIR>/WINDOWS/system32/config/system
```

Check for allowed open ports:
```
reglookup -p /ControlSet002/Services/SharedAccess/Parameters/FirewallPolicy/StandardProfile/GloballyOpenPorts <MOUNTED_DIR>/WINDOWS/system32/config/system
```

Check for authorized applications:
```
reglookup -p /ControlSet002/Services/SharedAccess/Parameters/FirewallPolicy/StandardProfile/AuthorizedApplications/List <MOUNTED_DIR>/WINDOWS/system32/config/system
```
### Windows logs ###

Get System event log:
```
evtextport -m all -t system -m all -t system -r <MOUNTED_DIR>/WINDOWS/system32/config/ <MOUNTED_DIR>/WINDOWS/system32/config/SysEvent.Evt
```

Get Security event log:
```
evtextport -m all -t security -m all -t security -r <MOUNTED_DIR>/WINDOWS/system32/config/ <MOUNTED_DIR>/WINDOWS/system32/config/SecEvent.Evt
```

Get Application event log:
```
evtextport -m all -t system -m all -t application -r <MOUNTED_DIR>/WINDOWS/system32/config/ <MOUNTED_DIR>/WINDOWS/system32/config/AppEvent.Evt
```
