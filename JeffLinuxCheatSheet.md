
# Getting Started


In order to troubleshoot App Service Linux issues, you'll need to understand the basic tools and how to use them.  Of course, you'll also need to know some Linux commands which are also available below.

[[_TOC_]]





# Tools


## External tools

- [Wappalyzer](https://www.wappalyzer.com/).
- [BuiltWith](https://builtwith.com/).
- Review file extensions in URL or inspect code in Web Developer tools.
- Check response headers for any additional information.



----

# Linux
App Service Linux uses Docker containers.  There are multiple Operating Systems that can be used within these containers but our blessed images will primarily use the following distributions.

- [Debian 9 (stretch)](https://wiki.debian.org/DebianStretch)
- [Alpine](https://alpinelinux.org/)

## Commands

Below are a list of commands and their descriptions used by Linux users.  They're categorized by common, network, and other.  The commands on Linux have the following syntax.

```bash
$command options arguments 
```

A full broader list and manuals can be found at https://linux.die.net/man/ 

### Common Commands
|Command|Description|
|---|---|
|sudo      |Allows users to run/execute commands as a superuser or another user with the necessary privileges.  
|ls        |Lists files
|cp        |Copies files
|mv        |Moves files
|touch     |Create files
|rm        |Remove files
|cat       |Display & concatenates files
|tail      |Displays last part of a file.  Default of 10 lines.
|grep      |Search based on patterns
|find      |Searches for a file
|mkdir     |Create directory
|rmdir     |Remove directory
|cd        |Change directory
|top       |Display Linux tasks
|ps        |Displays processes
|df        |Displays disk usage statistics
|clear     |Clear the screen. (use cls in bash)
|vi        |Text editor
|chown     |Used to change ownership of a file/directory.
|chmod     |Used to change access permissions (i.e. read-only, read-write, etc.) for a file/directory. 
|<command> --help|Flag used to display information about the command & expand upon usage.|

### Package Installation Commands

Each operating system will follow its own syntax for package installation. If unsure, the following command will print the release details for confirmation. Below we confirmed the OS as Debian.

```bash
$command cat/etc/os-release
```

![image.png](/.attachments/image-5a4ab94b-c282-46f5-84bc-e38fe944a032.png)
|Command|Description|Operating System|
|---|---|---|
|dpkg      |Tool used to install, build, remove Debian packages. More info [here](http://manpages.ubuntu.com/manpages/xenial/man1/dpkg.1.html)|Debian/Ubuntu
|apt-get   |Used to update, upgrade, install, remove packages. More info available [here](http://manpages.ubuntu.com/manpages/cosmic/man8/apt-get.8.html)|Debian/Ubuntu
|apt-cache |Used to search or show details for a package. More info available [here](https://linux.die.net/man/8/apt-cache)|Debian/Ubuntu
|apt       |Includes common commands from both 'apt-get" and 'apt-cache'. More info can be found [here](http://manpages.ubuntu.com/manpages/xenial/man8/apt.8.html)|Debian/Ubuntu
|apk       |Used to add, delete, fix, update, search, etc. packages on Alpine systems. More info can be found [here](https://wiki.alpinelinux.org/wiki/Alpine_Linux_package_management)|Alpine
|yum       |Used to update install, erase, etc. More info can be found [here](https://access.redhat.com/articles/yum-cheat-sheet)|Redhat/CentOS
|zypper    |Used to install, update, patch, remove, etc.  More info can be found [here](https://en.opensuse.org/SDB:Zypper_usage)|openSUSE

### Network Commands

|Command|Description|
|---|---|
|netstat        |Displays network statistics.  May require net-tools to be installed.
|ip address     |Displays IP Address for specified interfaces
|dig/nslookup   |Tool used to check DNS resolution and provides details for URL provided.
|iptables       |Used to block or allow traffic.
|curl           |Used to send and receive data from an endpoint
|tcpdump        |Tool used to capture network traffic.

### Other Commands
|Command|Description|
|---|---|
|echo        |Used to echo the value provided.  For example, "echo $TEST" will return the value of the $TEST variable.  Additionally, "echo "hello"" will return "hello".
|date        |Displays current time and date.
|man         |Provides manual for the application.
|touch       |Used to create empty files.
|file        |Returns the file type.
|stat        |Provides detailed information about a file.
|ln          |Creates a symbolic link to a file.  Similar to a shortcut.
|du          |Disk usage information of a file.
|df          |File system storage usage information.
|history     |List of commands recently used commands.
|env         |Shows environment variables and their values.
|lsof        |Lists of open files associated with an application.
|tree        |Tool used to display a directory tree structure.  Will need to be installed.


## System Structure & Directories

To help with viewing directory structure on a Linux file system, you can install a tool called "tree".  Below are the commands to install the tool.  

- Debian/Ubuntu

```bash
sudo apt-get update && sudo apt-get install tree
```
```bash
sudo apt update && sudo apt install tree
```
- RedHat/CentOS

```bash
sudo yum install tree
```
- openSUSE

```bash
sudo zypper install tree
```
- Alpine

```bash
apk add --update tree
```
Once you've installed "tree", you can run the following command to list the directory structure.

```bash
tree -L 1 /
```
This command will list only the main directories under "/".  Below is an example from an App Service on Linux app running PHP.

```bash
root@0a10342a6525:/home# tree -L 1 /
/
|-- appsvctmp
|-- bin
|-- boot
|-- dev
|-- etc
|-- home
|-- lib
|-- lib64
|-- media
|-- mnt
|-- opt
|-- proc
|-- root
|-- run
|-- sbin
|-- srv
|-- sys
|-- tmp
|-- usr
`-- var

20 directories, 0 files
root@0a10342a6525:/home#
```

**NOTE:**  If the operating system is running in a Docker container, "sudo" may not be necessary to install the package if you are accessing the container in interactive mode or through SSH.


|Directory|Description|
|---|---|
|/bin|Directory that contains binaries (some of the applications and programs you can run).| 
|/boot|The /boot directory contains files required for starting your system. DO NOT TOUCH!| 
|/dev|Contains device files. Many of these are generated at boot time or even on the fly. |
|/etc|Contains most, if not all system-wide configuration files. |
|/home|On App Service Linux, we mount the persisted file system to this path.|
|/lib|/lib is where libraries live. 
|/mnt |The /mnt directory, however, is a bit of remnant from days gone by. This is where you would manually mount storage devices or partitions. It is not used very often nowadays.|
|/opt|The /opt directory is often where software you compile sometimes lands. |
|/proc|/proc, like /dev is a virtual directory. It contains information about your computer, such as information about your CPU and the kernel your Linux system is running. |
|/root |/root is the home directory of the superuser (also known as the “Administrator”) of the system. |
|/run|System processes use it to store temporary data for their own nefarious reasons. This is another one of those DO NOT TOUCH folders.|
|/sbin|/sbin is similar to /bin, but it contains applications that only the superuser (hence the initial s) will need. |
|/usr |Another place where applications and libraries end up in is /usr/local, When software gets installed here, there will also be /usr/local/bin and /usr/local/lib directories.|
|/srv|The /srv directory contains data for servers. If you are running a web server from your Linux box, your HTML files for your sites would go into /srv/http (or /srv/www). If you were running an FTP server, your files would go into /srv/ftp.|
|/sys|Virtual directory like /proc and /dev and also contains information from devices connected to your computer.|
|/tmp|Contains temporary files, usually placed there by applications that you are running. |
|/var|Contains things like logs in the /var/log, web content under /var/www, etc.|


# Top

## Overview

Top is a Linux command that allows users to monitor system resources and processes and comes pre-installed.  This is a useful tool for troubleshooting performance issues and can be configured to display different views depending on the scenario.

## Top Interface - Summary

When running `top`, a summary is provided and contains useful information such as system time, uptime, user sessions, memory usage, CPU usage, number of tasks running, and the load average.  Below is an example of the top interface.

```bash
top - 22:15:15 up 2 days, 17:54,  1 user,  load average: 1.94, 1.21, 0.63
Tasks:  10 total,   1 running,   9 sleeping,   0 stopped,   0 zombie
%Cpu(s): 72.8 us, 23.8 sy,  0.0 ni,  0.0 id,  0.8 wa,  0.0 hi,  2.7 si,  0.0 st
KiB Mem :  1963824 total,   119336 free,   989932 used,   854556 buff/cache
KiB Swap:  1910780 total,  1798196 free,   112584 used.   689500 avail Mem

   PID USER      PR  NI    VIRT    RES    SHR S %CPU %MEM     TIME+ COMMAND
     1 root      20   0   17980   2732   2532 S  0.0  0.1   0:00.08 bash
    20 root      20   0   69960   2680   2156 S  0.0  0.1   0:00.00 sshd
    28 root      20   0    4280   1488   1392 S  0.0  0.1   0:00.01 startup.sh
    61 root      20   0  985872  40496  22732 S  0.0  2.1   0:00.69 npm
    71 root      20   0    4284    748    676 S  0.0  0.0   0:00.00 sh
    72 root      20   0  877876  30400  22948 S  0.0  1.5   0:00.21 node
    78 root      20   0 1301516 141552  24396 S  0.0  7.2   0:15.52 node
   113 root      20   0   69960   6108   5300 S  0.0  0.3   0:00.02 sshd
   115 root      20   0   18180   3088   2692 S  0.0  0.2   0:00.00 bash
   119 root      20   0   41020   3108   2652 R  0.0  0.2   0:00.00 top
```
### System Time and Up Time 

Below is a section from the top interface which provides details for the system time, uptime, and number of user sessions.

```bash
top - 22:15:15 up 2 days 17:54
```

The system time is `22:15:15` and has been up for 2 days, 17 hours and 54 minutes.  

### User Sessions

```bash
top - 22:15:15 up 2 days 17:54,  1 user
```

Next to the up time, you can find the number of user sessions.  If there is more than 1 user session, you can use the `who` command to find details about the other user sessions.  Example provided below showing to sessions opened by the root user.

```bash
root@6b9d0d4e23d7:/home# who
root     pts/0        May 19 22:13 (172.16.7.2)
root     pts/1        May 19 22:21 (172.16.7.2)
```

Users can login to systems either physically, through a terminal emulator, or SSH.  The `who` output will show **TTY** for users that are physically signed in, **PTY** and **PTS** are for sessions created through a terminal emulator or SSH.  You'll notice that there is `pts` next to the user.  PTS is an acronym for *pseudo terminal slave* is a slave part of a PTY.

### Load Average



The next section of the top summary will display the load average on the system.  The average load is the number of processes in *R* and *D* states and displayed in 1, 5, and 15 minute time frames.  This is helpful for performance related issues as it can tell us if the system is overloaded.

```bash
top - 22:15:15 up 2 days, 17:54,  1 user,  load average: 1.94, 1.21, 0.63
```
In the example above, you can see that the load average for the last minute is **1.94**.  On a single core system, the max load will be **1**.  This means that the system is overloaded by 94% and will take some time to perform additional tasks.  However, we can also see that the 5 minute average is **1.21** which means that there was a recent task that increased the load but is averaging 1.21.  On multi-core systems, you'll need to factor the number of cores to calculate the load.  

### Tasks

```bash
Tasks:  10 total,   1 running,   9 sleeping,   0 stopped,   0 zombie
```
The tasks sections provides a quick summary of the total number of tasks and their states.  For more information regarding the task states, go the *Task* section below.

### CPU Usage

```bash

%Cpu(s): 72.8 us, 23.8 sy,  0.0 ni,  0.0 id,  0.8 wa,  0.0 hi,  2.7 si,  0.0 st
```
CPU state percentages based on the interval since the last refresh.

As a default, percentages for these individual categories are displayed.  Where two labels are shown below, those for more recent kernel versions are shown first.

- us, user    : time running un-niced user processes
- sy, system  : time running kernel processes
- ni, nice    : time running niced user processes
- id, idle    : time spent in the kernel idle handler
- wa, IO-wait : time waiting for I/O completion
- hi : time spent servicing hardware interrupts
- si : time spent servicing software interrupts
- st : time stolen from this vm by the hypervisor

### Memory Usage

```bash

KiB Mem :  1963824 total,   119336 free,   989932 used,   854556 buff/cache
KiB Swap:  1910780 total,  1798196 free,   112584 used.   689500 avail Mem
```

This portion consists of two lines which may express values in kibibytes (KiB) through exbibytes (EiB) depending on the scaling factor enforced with the `E' interactive command. As a default, Line 1 reflects physical memory, classified as:
   
    total, free, used and buff/cache

Line 2 reflects mostly virtual memory, classified as:
    total, free, used and avail (which is physical memory)

The avail number on line 2 is an estimation of physical memory available for starting new applications, without swapping.  Unlike the free field, it attempts to account for readily reclaimable page cache and memory slabs.  It is available on kernels 3.14, emulated on kernels 2.6.27+, otherwise the same as free.




## Top Interface - Task Area

```bash

   PID USER      PR  NI    VIRT    RES    SHR S %CPU %MEM     TIME+ COMMAND
     1 root      20   0   17980   2732   2532 S  0.0  0.1   0:00.08 bash
    20 root      20   0   69960   2680   2156 S  0.0  0.1   0:00.00 sshd
    28 root      20   0    4280   1488   1392 S  0.0  0.1   0:00.01 startup.sh
    61 root      20   0  985872  40496  22732 S  0.0  2.1   0:00.69 npm
    71 root      20   0    4284    748    676 S  0.0  0.0   0:00.00 sh
    72 root      20   0  877876  30400  22948 S  0.0  1.5   0:00.21 node
    78 root      20   0 1301516 141552  24396 S  0.0  7.2   0:15.52 node
   113 root      20   0   69960   6108   5300 S  0.0  0.3   0:00.02 sshd
   115 root      20   0   18180   3088   2692 S  0.0  0.2   0:00.00 bash
   119 root      20   0   41020   3108   2652 R  0.0  0.2   0:00.00 top

```

### PID - Process Id

Unique ID used to identify a process/task.

### USER - User Name

The username that is mapped to a user ID.  On Linux, different services such as a web server may run under a user ID such as *www-data* with limited privileges to prevent security issues.  

### PR - Priority

The scheduling priority of the task.  If you see `rt' in this field, it means the task is running under real time scheduling priority.  Under linux, real time priority is somewhat misleading since traditionally the operating itself was not preemptible.  And while the 2.6 kernel can be made mostly preemptible, it is not always so.

### VIRT, RES , SHR and %MEM

These three fields are related with to memory consumption of the processes. 

- VIRT - Virtual Memory Size (KiB):  is the total amount of memory consumed by a process. This includes the program’s code, the data stored by the process in memory, as well as any regions of memory that have been swapped to the disk. 

- RES - Resident Memory Size (KiB): is the memory consumed by the process in RAM.

- %MEM - Memory Usage (RES): expresses this value as a percentage of the total RAM available. 

- SHR - Shared Memory Size (KiB): is the amount of memory shared with other processes.

### S - Process Status

The status of the task which can be one of:
D = uninterruptible sleep
I = idle
R = running
S = sleeping
T = stopped by job control signal
t = stopped by debugger during trace
Z = zombie

### TIME+ - CPU Time, hundredths
Total CPU time the task has used since it started., precise to the hundredths of a second.

### COMMAND - Command Name or Command Line

Display the command line used to start a task or the name of the associated program.  You toggle between command line and name with `c', which is both a command-line option and an interactive command.

When you've chosen to display command lines, processes without a command line (like kernel threads) will be shown with only the program name in brackets, as in this example:
             
    [kthreadd]

## Top Interface Usage

### Sorting Processes

You type the following options to sort the process/tasks lists.

- "M" - Sort by memory usage
- "P" - Sort by CPU usage
- "N" - Sort by process ID
- "T" - Sort by the running time

**NOTE:** These commands are case-sensitive.


### Killing Processes

Press "k" and enter the PID that you would like to kill and press enter.  
```bash

top - 22:18:45 up 1 day, 17:58, 11 users,  load average: 0.74, 0.97, 0.62
Tasks:  29 total,   1 running,  28 sleeping,   0 stopped,   0 zombie
%Cpu(s):  5.1 us,  5.8 sy,  0.0 ni, 76.8 id, 12.3 wa,  0.0 hi,  0.0 si,  0.0 st
KiB Mem :  1963824 total,   131272 free,  1099352 used,   733200 buff/cache
KiB Swap:  1910780 total,  1711828 free,   198952 used.   573604 avail Mem
PID to signal/kill [default pid = 78] 165
   PID USER      PR  NI    VIRT    RES    SHR S %CPU %MEM     TIME+ COMMAND
    78 root      20   0 1297148  84684   3452 S  0.0  4.3   0:16.53 node
    61 root      20   0  985872   7652    396 S  0.0  0.4   0:00.69 npm
    72 root      20   0  877876   5852    364 S  0.0  0.3   0:00.21 node
   176 root      20   0   69960   5596   4792 S  0.0  0.3   0:00.22 sshd
   178 root      20   0   18180   3244   2708 S  0.0  0.2   0:00.01 bash
   184 root      20   0   41040   3024   2588 R  0.3  0.2   0:00.30 top
   165 root      20   0   69960   1372    568 S  0.0  0.1   0:00.01 sshd

```
After you press enter, you'll see the following output which will use SIGTERM to gracefully kill the process.  Press enter to continue.

```bash
top - 22:18:45 up 1 day, 17:58, 11 users,  load average: 0.74, 0.97, 0.62
Tasks:  29 total,   1 running,  28 sleeping,   0 stopped,   0 zombie
%Cpu(s):  5.1 us,  5.8 sy,  0.0 ni, 76.8 id, 12.3 wa,  0.0 hi,  0.0 si,  0.0 st
KiB Mem :  1963824 total,   131272 free,  1099352 used,   733200 buff/cache
KiB Swap:  1910780 total,  1711828 free,   198952 used.   573604 avail Mem
Send pid 165 signal [15/sigterm]
```

If you need to forcefully kill the process, type "SIGKILL" and press enter.  You can also type "9" for SIGKILL or "15" for SIGTERM.

### Filtering Processes

If you have a lot of processes running, you can filter any of the columns provided in the task view by pressing "o" or "O".  The following will appear in the top interface.

```bash
add filter #1 (ignoring case) as: [!]FLD?VAL
```

Below are some samples that you can use.

- Filter by Command:  `COMMAND=<command_name>` or `!COMMAND=<command_name>`
- Filter by %CPU, %MEM, etc.: `%MEM>2.0

To clear the filters, press "=".

### References

For more information about the top, please reference the manpages at http://man7.org/linux/man-pages/man1/top.1.html

# iostat

**iostat** is a tool used to help isolate performance issues which may be caused by system performance issues.  Many different open source frameworks may make more read/write operations on the file system than others.  This tool can help with verifying system performance is impacting the overall application performance.

## Installing iostat


`iostat` isn't installed by default on some Linux OS's.  To install iostat, use the following commands based on your Linux distribution.

### Ubuntu/Debian

```bash
apt-get update && apt-get install sysstat
```

### CentOS

```bash
yum install sysstat
```

### Alpine

```bash
apk update && apk add sysstat
```
## Usage

After installing the sysstat package, simply type `iostat` to get a report and statistics.

```bash
root@6b9d0d4e23d7:/home# iostat
Linux 4.4.0-128-generic (6b9d0d4e23d7)  05/20/20        _x86_64_        (1 CPU)

avg-cpu:  %user   %nice %system %iowait  %steal   %idle
           5.17    0.01    4.52    6.46    0.00   83.83

Device:            tps    kB_read/s    kB_wrtn/s    kB_read    kB_wrtn
loop0            17.89        63.80       213.58    9633081   32246496
fd0               0.00         0.00         0.00          8          0
sda              18.59        46.64       222.77    7041593   33633252
sdb               3.03        76.64       214.48   11570464   32381892
sdc               0.02         1.42         1.02     213841     154536
```
The `avg-cpu` section contains the following information.

- %user : Percentage of CPU being utilization that while executing at the user level.
- %nice : Percentage of CPU utilization that occurred while executing at the user level with a nice priority.
- %system : Percentage of CPU utilization that occurred while executing at the system (kernel) level.
- %iowait : Percentage of the time that the CPU or CPUs were idle during which the system had an outstanding disk I/O request.
- %steal : Percentage of time being spent in involuntary wait by the virtual CPU or CPUs while the hypervisor was servicing by another virtual processor.
- %idle : Percentage of time that the CPU or CPUs were idle and the system did not have an outstanding disk I/O request.

The other remaining section provides additional information for disk performance.

- Device : The device/partition name is listed in /dev directory.
- tps : Transfers per second that were issued to the device. Higher tps means the processor is busier.
- Blk_read/s : Shows the amount of data read from the device expressed in a number of blocks (kilobytes, megabytes) per second.
- Blk_wrtn/s : The amount of data written to the device expressed in a number of blocks (kilobytes, megabytes) per second.
- Blk_read : Total number of blocks read.
- Blk_wrtn : Total number of blocks written.

Additional options can be used to show more information or filter the report & statistics.  Below are the commands with descriptions.

- iostat: Get report and statistic.
- -x: Show more details statistics information.
- -c: Show only the cpu statistic.
- -d: Display only the device report.
- -xd: Show extended I/O statistic for device only.
- -k: Capture the statistics in kilobytes or megabytes.
- -k 2 3: Display cpu and device statistics with delay.
- -j ID mmcbkl0 sda6 -x -m 2 2: Display persistent device name statistics.
- -p: Display statistics for block devices.
- -N: Display lvm2 statistic information.


