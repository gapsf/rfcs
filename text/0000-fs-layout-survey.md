- Feature Name: fs-layout-survey
- Start Date: 2017-06-17
- RFC PR: (leave this empty)
- Redox Issue: (leave this empty)

# Summary
[summary]: #summary
Suggests clear separation of OS files, applications files, configuration files, user's data files.

# Motivation
[motivation]: #motivation
## Configuration and DE's files in home directory
In GNU/Linux and Windows user's home directories are polluted by myriads of configuration directories and files created
by programs.
Abstractions that hide filesystem from the user, like DE's 'Desktop' and configuration also lays down directly 
in user's home directory.
**So user does not have a clean place to store his/her own data.**

To overcome this problem those files are "hidden" from users to pretend they don't exists since ordinary users shouldn't see o touch these files.
"Hidden file" concept is alogical and counterintuitive - file should be placed in the "correct" place, not hidden.

Also users partially forced to store own data files in files hierarhy predefined by DE.
It is inconvenient for power/exprienced users and admins to manage his/her files home directory with file managers.

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
Minimal set of files to manage OS from CLI, like: kernel, drivers, shell; disk, networking, keyboard utilities and
so on.
## OS configuration files
Files to configure kernel parameters or parameters related to resources, managed by kernel only: disks, networking, memory, processes and hardware.
## User configuration and data files
User's home directory should be always clean from any user's, system's and application's configuration files or files like DE's 'Desktop' directory and should not force any hierarchy inside.
There should not be a concept like "hidden file" to hide those files existence from user.

*All* user configuration should be stored:
- in the dedicated directory outside the user's home directory;
- in the directory in user home directory.


So hierarchy could be like:
```
/
    /boot 
    /os                     - OS files only;
        /kernel             - OS kernels;
        /drivers            - OS drivers;
    /cfg
        /sys                - host's configuration files (OS parameters, networking, disks, host.conf, hosts.allow, ...)
        /soft               - systemwide applications configuration files;
        /users              - users configuration and special files, like DE files, shell, ssh-keys and so on...
            /alice
            /bob
    /soft                   - systemwide binares and apps;
        /apps               - preinstalled and users applications in directories with arbitrary names;
            /firefox-v1
            /libreoffice-v1
            /firefox-v2
            /libreoffice-v2
        /bin                - preinstalled and users binaries;
        /lib
    /users                  
        /home               - users home directories for user's data
            /root
            /alice           
            /bob
    /run
    /tmp
        /alice
        /bob
```
For multiple OS/application 
# Drawbacks
[drawbacks]: #drawbacks

Why should we *not* do this?

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
