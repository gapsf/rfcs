- Feature Name: fs-layout-survey
- Start Date: 2017-06-17
- RFC PR: (leave this empty)
- Redox Issue: (leave this empty)

# Summary
[summary]: #summary
Suggests clear separation of native Redox-OS files, applications files, configuration files, user's data files.

# Motivation
[motivation]: #motivation
## Configuration and DE's files in home directory
In GNU/Linux and Windows user's home directories are polluted by myriads of configuration directories and files created
by programs.
Abstractions that hide filesystem from the user, like DE's 'Desktop' and configuration also lays down directly 
in user's home directory.
**So user does not have a clean place to store his/her own data.**

To overcome this problem those files are "hidden" from users to pretend they don't exists since ordinary users shouldn't see o touch these files.
"Hidden file" concept is illogical and counter intuitive - file should be placed in the "correct" place, not hidden.

Also users partially forced to store own data files in files hierarchy predefined by DE.
It is inconvenient for power/experienced users and admins to manage his/her files home directory with file managers.

## OS/host and application configuration
In GNU/Linux and Windows there is no clear distinguish between system and application configuration files.

## Multiversion OS and application installation
In GNU/Linux  use of different versions of the software at the same time is very difficult.
It's badly supported by Filesystem Hierarchy Standard and package managers.
So installation and usage different versions of the same software (including OS versions) should be possible and easy.

# Detailed design
[design]: #detailed-design
All files must belong to one of the categories and should be located in the corresponding directory:
- boot loader;
- OS files;
- OS configuration files;
- drivers;
- applications files; 
- applications global configuration files; 
- users specific configuration files;
- users data;
- temporary files;
- run-time files.

## OS files
Minimal set of files to manage OS subsystems from CLI like: kernel, drivers, shell; disk, networking, keyboard utilities,
low level utilities hardware and package manager (If any).

## OS configuration files
Files to configure kernel parameters or parameters related to resources, managed by kernel only: disks, networking, memory, processes and hardware.

## Applications files it's configuration files
All other software like: servers, DE's, browsers, office suits, games, utilities and etc.
Should be in under separate directory.

## User home directory configuration and data files
User's home directory should be always clean from any user's, system's and application's configuration files or files like DE's 'Desktop' directory and should not force any hierarchy inside.
There should not be a concept like "hidden file" to hide those files from user.

## User-specific  configuration files
There is two options where user configuration files should be stored:
- in the dedicated directory outside the user's home directory **or**
- in the exactly **one** dedicated directory under user's home directory.

## Without multiversioning support hierarchy could be like:
```
/
    /boot
    /os                     - OS files only;
        /cfg                - OS and management configuration files;
        /drivers            - OS drivers;
        /kernel             - OS kernels;
        /bin                - OS management soft;
        /lib
    /cfg
        /sys                - host's  (OS parameters, networking, disks, host.conf, hosts.allow, ...)
        /users              
            /alice
            /bob
    /soft                   - system wide binares and apps;
        /cfg                - system wide apps configuration;
        /apps               - applications in directories with arbitrary names;
            /firefox-v1
            /libreoffice-v1
            /firefox-v2
            /libreoffice-v2
            /web-server-v1
            /web-server-v2
            /ftp-server-v1
            /ftp-server-v2
        /bin                - non-OS binaries;
        /lib
    /users                  - users home directories for user's data
        /alice
            /cfg            - user's configuration and special files, like DE files, shell, ssh-keys and so on...
        /bob
            /cfg            
    /run
    /tmp
        /alice
        /bob
```
## With multiversioning support hierarchy could be like:

# Drawbacks
[drawbacks]: #drawbacks


# Alternatives
[alternatives]: #alternatives
In abstracted directories like DE's Desktop there is no clear separation between configuration and data.
So users hierarchy could be like:
```
/users                  
    /alice          - users home directories for user's data
        /cfg        - users configuration and special files, like DE files and dirs, shell, ssh-keys and so on...
    /bobe
        /cfg        

```
# Unresolved questions
[unresolved]: #unresolved-questions

Where store files copied on Desktop?
