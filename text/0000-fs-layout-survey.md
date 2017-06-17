- Feature Name: fs-layout-survey
- Start Date: 20017-06-17
- RFC PR: (leave this empty)
- Redox Issue: (leave this empty)

# Summary
[summary]: #summary
Proposes clear separation of:
- OS files;
- OS configuration files;
- application files; 
- application configuration files; 
- user's environment configuration files;
- user's data;
- run-time files.

# Motivation
[motivation]: #motivation
## Windows
In Windows, system directory (most often 'c:\windows') constantly changing and polluted with different sort of files
like app's shared libraries, binaries and other type of files.
It's mostly impossible to copy OS configuration from one instance to another.

User's home directories (aka 'profiles') are polluted by myriads of configuration directories and files which are hidden
from ordinary users to pretend they don't exists and all it's configuration stuff 'magically' stored somewhere.
Abstractions that hide filesystem from the user, like DE's 'Desktop' and configuration also lays directly 
in user's home directory.

## GNU/Linux
Application files spreads over whole FS hierarchy and having multiple versions of some software a very difficult or impossible.

User's home directories (/home/*) are also polluted by configuration directories and files which are hidden
from ordinary users to pretend they don't exists.

All this issues make it difficult to manage the system by power users and admins.

# Detailed design
[design]: #detailed-design
All files must belong to one of the categories:
- boot loader;
- OS(s) files;
- OS(s) configuration files;
- drivers;
- applications files; 
- applications global configuration files; 
- users specific configuration files;
- users data;
- temporary files;
- run-time files.

Files of one category should be located in the corresponding directory only.
As option, it would be nice to be able to install multiple versions of RedoxOS and applications.
There should not be a concept like "hidden file".
Temporary files should be stored per user.
User's home directory should be clean from any "system or application" files and should not force any hierarchy inside.

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
