# **Practical Session: Execute Basic Linux Commands on Ubuntu Server**

### **🎯 Objective**
By the end of this session, you will be able to:  
✅ Connect to a cloud server (Ubuntu VPS)  
✅ Use essential Linux commands for directory, file, and permission management  
✅ Submit your work by creating a confirmation file on the server  


This section lists essential Linux commands you should become familiar with as part of your cloud computing practicals. These commands help with navigating the file system, editing files, managing permissions, monitoring logs, and understanding system directories.

---

## 📁 File and Directory Navigation

| Command | Description |
|--------|-------------|
| `ls` | List directory contents |
| `ls -al` | List all files (including hidden) with details |
| `cd` | Change directory |
| `pwd` | Print working directory |
| `mkdir dirname` | Create a new directory |
| `rmdir dirname` | Remove an empty directory |
| `touch filename` | Create an empty file |
| `cp src dest` | Copy files/directories |
| `mv src dest` | Move or rename files/directories |
| `rm filename` | Remove a file |
| `rm -r dirname` | Remove a directory recursively |
| `find . -name "*.log"` | Find all `.log` files in current directory and subdirectories |

---

## 🔐 File Permissions & Ownership

| Command | Description |
|--------|-------------|
| `chmod 755 file` | Set permissions (rwxr-xr-x) |
| `chmod +x script.sh` | Make script executable |
| `chown user file` | Change ownership of file |
| `chown user:group file` | Change owner and group |
| `ls -l` | View file permissions and ownership |
| `umask` | Show default permission mask |
| `stat filename` | Show detailed info about a file |

---

## 📂 Important Directories in Ubuntu

| Directory | Purpose |
|----------|---------|
| `/home` | User home directories |
| `/etc` | System-wide configuration files |
| `/var/log` | Log files |
| `/tmp` | Temporary files |
| `/usr` | User programs, libraries |
| `/bin` | Essential binaries |
| `/sbin` | System binaries (for root/admin) |
| `/dev` | Device files |
| `/proc` | Kernel and process info (virtual FS) |
| `/boot` | Boot loader files |
| `/lib` | Shared libraries |
| `/mnt` | Temporary mounted filesystems |
| `/opt` | Optional software packages |

---

## 📝 File Viewing and Editing

| Command | Description |
|--------|-------------|
| `cat file` | View file contents |
| `less file` | Scroll through large files |
| `more file` | View file page-by-page |
| `head -n 20 file` | Show first 20 lines |
| `tail -n 20 file` | Show last 20 lines |
| `nano file` | Edit file using Nano editor |
| `vim file` | Edit file using Vim editor |
| `echo "text" > file` | Write text to file (overwrite) |
| `echo "text" >> file` | Append text to file |

---

## 📈 Monitoring Logs and Processes

| Command | Description |
|--------|-------------|
| `tail -f /var/log/syslog` | View live system log |
| `tail -f /var/log/nginx/access.log` | View live access logs (example) |
| `dmesg` | View kernel messages |
| `top` | Real-time system usage |
| `htop` | Enhanced top (if installed) |
| `ps aux` | List all running processes |
| `kill PID` | Kill a process by PID |
| `df -h` | View disk usage |
| `du -sh folder` | Check folder size |

---

## 📦 Package and System Updates

| Command | Description |
|--------|-------------|
| `sudo apt update` | Update package lists |
| `sudo apt upgrade` | Upgrade all packages |
| `sudo apt install package` | Install new package |
| `sudo apt remove package` | Remove a package |
| `sudo reboot` | Restart the server |
| `sudo shutdown now` | Shut down the server immediately |

---

## **🚀 Instructions**

Follow each step in sequence and make sure each command executes without error. At the end, you will generate a confirmation file and submit it as proof of completion.

---

📸 Take a screenshot of each step you perform in the terminal, including the displayed file content.
Save all screenshots in a single PDF file and submit it to samadhivkcom@gmail.com.

### **Step 1: Connect to the VPS**
1. Connect to your Ubuntu server using SSH:
```sh
ssh root@your_server_ip
```

---

### **Step 2: Setup Working Directory**
2. Create a directory named `linux-practical` in your home folder:
```sh
mkdir ~/linux-practical
```

3. Navigate into this directory:
```sh
cd ~/linux-practical
```

---

### **Step 3: Create and Manage Files**
4. Create three empty files:
```sh
touch file1.txt file2.txt file3.txt
```

5. Append some text to each file:
```sh
echo "This is file1" > file1.txt
echo "This is file2" > file2.txt
echo "This is file3" > file3.txt
```

---

### **Step 4: Permission Handling**
6. Change the permissions of `file1.txt` to be readable and writable by owner only:
```sh
chmod 600 file1.txt
```

7. Make `file2.txt` executable by all users:
```sh
chmod a+x file2.txt
```

---

### **Step 5: File Content & Viewing**
8. View the contents of `file3.txt` using `cat`:
```sh
cat file3.txt
```

9. View a detailed list of all files and permissions in the current directory:
```sh
ls -al
```

---

### **Step 6: System Monitoring**
10. View system memory and usage with:
```sh
top
```
> Press `q` to exit `top`.

11. Check your current disk space:
```sh
df -h
```

---

### **Step 7: Logs & Processes**
12. View the last 5 lines of the system log:
```sh
tail -n 5 /var/log/syslog
```

13. Show all currently running processes:
```sh
ps aux
```

---

### **Step 8: Submit Proof of Work**
14. Create a final file as submission proof:
```sh
echo "Student [Your Name] completed the Linux practical on $(date)" > ~/linux-practical/submission.txt
```

15. Show that your file was created with this command:
```sh
cat ~/linux-practical/submission.txt
```

---

## ✅ You’re Done!
You’ve successfully executed and demonstrated basic Linux skills on a remote Ubuntu server. Well done!

---



