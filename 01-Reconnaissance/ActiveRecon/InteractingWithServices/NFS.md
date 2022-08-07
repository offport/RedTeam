## NFS - Cheatsheet

#### Discovery - Scanning for NFS Service 

List the directories shared in the host

	showmount -e <ip>

Or scan

	nmap -p 111 --script nfs* <ip> 

---

#### Mount and Unmount

Create a directory to mount to 

	mkdir <directory>
	
Mount	

	sudo mount -o nolock <ip>:/<shared drive> ~/<directory>/

	cd <directory> && ls

Unmount

	umount ~/<directory>

---

#### Metasploit

`use auxiliary/scanner/nfs/nfsmount`
	
	
#### References:
	
- https://resources.infosecinstitute.com/topic/exploiting-nfs-share/
- https://www.rapid7.com/db/modules/auxiliary/scanner/nfs/nfsmount/