### Copy a Device UUID from CLI

Use blkid to find the UUID (requires root priveleges):  
`blkid -s UUID -o value /dev/device/name`

Use lsblk:  
`lsblk -o UUID /dev/device/name | grep -v UUID`

The output can then be used appended to a file:  
`lsblk -o UUID /dev/device/name | grep -v UUID >> file.txt`