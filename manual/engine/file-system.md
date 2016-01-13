# File System

The static class @'SiliconStudio.Core.IO.VirtualFileSystem' is the recommended way to access files in a cross-platform manner.

It offers all basic operations such as reading, writing, copying, checking existence and deleting files.

> **Note**
> 
> Path separator is / (Unix/Linux convention).    

# Usage

```cs
// Open a file through VirtualFileSystem
var gamesave1 = VirtualFileSystem.OpenStream("/roaming/gamesave001.dat", VirtualFileMode.Open, VirtualFileAccess.Read);
 
// Alternatively, you can directly access the same file through its file system provider (mount point)
var gamesave2 = VirtualFileSystem.ApplicationRoaming.OpenStream("gamesave001.dat", VirtualFileMode.Open, VirtualFileAccess.Read);```




# Default mount points

| Mount point | Description                               | Writable | Cloud | Notes                                                           | PC                       | Android                        | iOS                        | Windows Phone 8.1      |
| ----------- | ----------------------------------------- | -------- | ----- | --------------------------------------------------------------- | ------------------------ | ------------------------------ | -------------------------- | ---------------------- |
| data        | Application data, deployed by package     | ✗        | ✗     |                                                                 | Output directory/data    | APK itself                     | Deployed package directory | InstalledLocation.Path |
| binary      | Application binaries, deployed by package | ✗        | ✗     | Usually same as app_data (except Android)                       | Assembly directory       | Assembly directory             | Assembly directory         | Assembly directory     |
| roaming     | User specific data (roaming)              | ✓        |  ✓    | Backup                                                          | Output directory/roaming | $(Context.getFilesDir)/roaming | Library/Roaming            | Roaming                |
|             |                                           |          |       |                                                                 |                          |                                |                            |                        |
|             |                                           |          |       |                                                                 | %APPDATA%                |                                |                            |                        |
| local       | User application data                     | ✓        |  ✓    | Backup                                                          | Output directory/local   | $(Context.getFilesDir)local    | Library/Local              | Local                  |
|             |                                           |          |       |                                                                 |                          |                                |                            |                        |
|             |                                           |          |       |                                                                 |                          | SDCard media?                  |                            |                        |
| cache       | Application cache                         | ✓        | ✗     | DLC, etc...                                                     | Output directory/cache   | $(Context.getFilesDir)/cache   | Library/Caches             | LocalCache             |
|             |                                           |          |       |                                                                 |                          |                                |                            |                        |
|             |                                           |          |       | Might be deleted manually by user (restore, clear data, etc...) |                          | SDCard media?                  | with do not backup flags   |                        |
| tmp         | Application temporary data                | ✓        | ✗     | Might be deleted without notice by OS                           | Output directory/temp    | $(Context.getCacheDir)         | tmp                        | Temporary              |
|             |                                           |          |       |                                                                 |                          |                                |                            |                        |
|             |                                           |          |       |                                                                 | %TEMP%/%APPNAME%         |                                |                            |                        |


# Specific considerations

- On Android, we should offer ability to have roaming and/or local exposed on SDCard if user wants it (so that backup/copy of data is easy).
  
  Or maybe it could be alternative mount point?

 

