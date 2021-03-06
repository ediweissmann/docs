## Spoon Studio

Using Spoon Studio enables you to configure the filesystem and registry of a virtual application, embed external runtimes and components, take application snapshots, and create Spoon Virtual Machine (SVM) or executable files. The primary interface consists of a ribbon bar along the top and several panes accessed by buttons on the left.

**Title Bar**

- The Start Menu button, or the circle at the top left of the window, enables virtual application configurations to be created, opened, saved, imported, applied, and closed.
- The Options bar provides Spoon Studio customization options, the ability to set proxy settings, and install certificates.
- The Help bar provides access to Spoon Studio documentation.

**Ribbon Bar**

- The Virtual Application tab provides access to the snapshot, build features, and output configuration options such as the startup file, output directory, and diagnostic-mode selection.
- The Runtimes tab provides a selection of auto-configurable runtime engines which can be embedded into your application. These include .NET Framework, Java, Flash, Shockwave, Acrobat Reader, and SQL Server 2005 Express.
- The Advanced tab provides functions such as Platform Merge and Streaming.

**Left-side Button Panes**

- The **Filesystem** pane displays the application virtual filesystem and enables you to add and remove virtual files and directories.
- The **Registry** pane displays the application virtual registry and enables you to add and remove virtual registry keys and data values.
- The **Settings** pane enables you to configure virtual application metadata, startup image, startup/shutdown shims, and process configuration options.
- The **Network** pane enables you to configure virtual application proxy and DNS settings.
- The **Components** pane enables you to add external virtual application components.
- The **Setup** pane enables you to configure the MSI setup package, shortcuts, and other shell integration options.
- The **Expiration** pane enables you to configure application expiration options.

**Note:** Spoon Studio users are responsible for any third-party licensing compliance for redistributable components included using virtualization.

### Virtual Filesystem

Spoon Studio enables you to embed a *virtual filesystem* into your executable. Embedded files are accessible by your Spoon-processed application as if they were present in the actual filesystem. Virtual files are isolated from the host device. Virtual files do not require security privileges on the host device regardless of whether the virtual files reside in a privileged directory. Because virtual files are embedded in the application executable, shared DLLs do not interfere with those installed by other applications on the host device. 

In the event of a conflict between a file in the virtual filesystem and a file present on the host device, the file in the virtual filesystem takes precedence.

**Note:** When running a virtual application on Windows 7, the **All Users Directory\Application Data** and **All Users Directory** root folders will map to the same folder at runtime. To prevent one setting from overriding another, verify that the isolation settings for these folders are the same.

**Isolation Modes**

Folders may be virtualized in **Full**, **Merge**, **Write Copy**, or **Hide** mode.

- **Full**: Full mode is used when the directory is required to be completely isolated from the host device. Only files in the virtual filesystem are visible to the application even if a corresponding directory exists on the host device. Any writes to files are redirected to the sandbox data area. 
- **Merge**: Merge mode is used when the directory is required to have some level of interaction with the host device. Files will be visible from both the virtual filesystem and the host device. Any writes to files which are new or already exist on the host device will pass through and be written to the host device. Any writes to files that are in the virtual filesystem will be written to the sandbox data area.
- **Write Copy**: Write Copy mode is used when the directory is required to have visibility to files on the host device but cannot change them. Files will be visible from both the virtual filesystem and the host device. Any writes to files are redirected to the sandbox data area. 
- **Hide**: Hide mode is used when a directory on the host device could interfere with the application's ability to run properly. The application will receive a File Not Found error code whenever an attempt is made to read from or write to files in the directory even if the files exist on the host device.  

**Tip**: To apply selected isolation modes to all subfolders, right-click on the folder, choose Isolation, select the checkbox for **Apply to Subfolders**, then select **OK**.

**File Attributes**

**Hidden**

Files and folders can be hidden from shell browse dialogs and other applications. This is used to prevent internal components and data files from being displayed to the user. To hide a file or folder, select the checkbox in the **Hidden** column adjacent to the desired file or folder.

**Note**: The **Hidden Flag** is NOT the same as the **Hide** isolation mode. Enabling the **Hidden Flag** prevents a file or folder from displaying in browse dialogs or from directory enumeration APIs; it does not prevent the application (and potentially the end-user) from accessing the folder or file contents by direct binding. To prevent the file or folder from being found by the application, enable **Hide** isolation mode.

**Read Only**

Flagging files and folders as read-only prevents the application from modifying the file or folder contents. To make a file or folder read-only, select the checkbox in the **Read Only** column next to the desired file or folder.

**No Upgrade**

By default, files in the virtual filesystem can be upgraded with patches (refer to "Updating Registration Settings" in the **Register Virtual Applications in the Windows Shell** section for more information). If there are files in the virtual filesystem that should not be upgraded, such as user data files, select the checkbox in the **No Upgrade** column next to the desired file or folder.

**No Sync**

This feature only applies to virtual applications that are delivered and managed by Spoon Virtual Desktop Server. By default, files in the virtual filesystem can be synchronized to a user's Spoon account. This enables the application state to be maintained across different devices that are Spoon enabled. If there are folders in the virtual filesystem that should not be synchronized and remain only on the local device, select the checkbox in the **No Sync** column next to the desired file or folder. This setting is managed on a folder level and applies to all files within that folder.

**Filesystem Compression**

To reduce executable size, Spoon Studio can compress virtual filesystem contents. This reduces virtual application size by approximately 50% but also prevents profiling and streaming of the application. By default, the **Compress Payload** option in the **Process Configuration** area of the **Settings** panel is unchecked. Leave this box unchecked during the build process if the application will be optimized for streaming from Spoon Virtual Desktop Server.

**Note**: Disabling payload compression may significantly increase the size of the virtual application binary.

### Virtual Registry

Spoon Studio enables you to embed a virtual registry into your executable. Embedded registry keys are accessible by your Spoon-processed application as if they were present in the actual registry. Virtual registry keys are isolated from the host device. Virtual registry keys do not require security privileges on the host device regardless of whether the virtual files reside in a privileged directory. Because virtual registry entries are embedded in the application executable, other applications are unable to disrupt application execution by inadvertent modification of registry entries.

The **Classes** root, **Current User** root, **Local Machine** root, and **Users** root folders correspond to the **HKEY\_CLASSES\_ROOT**, **HKEY\_CURRENT\_USER**, **HKEY\_LOCAL\_MACHINE**, and **HKEY\_USERS** keys on the host machine. 

Registry string values can include well-known variables such as **@WINDIR@**, **@SYSWOW64@**, **@PROGRAMFILESX86@** and **@PROGRAMFILES@**.

In the event of a conflict between a key or value in the virtual registry and data present on the host device registry, information in the virtual registry takes precedence. 

**Isolation Modes**

Keys may be virtualized in **Full**, **Merge**, **Write Copy**, or **Hide** mode.

- **Full**: Full mode is used when the key is required to be completely isolated from the host device. Only values in the virtual registry are visible to the application even if a corresponding key exists on the host device. Any writes to values are redirected to the sandbox data area. 
- **Merge**: Merge mode is used when the key is required to have some level of interaction with the host device. Values will be visible from both the virtual registry and the host device. Any writes to values which are new or already exist on the host device will pass through and be written to the host device. Any writes to values that are in the virtual registry will be written to the sandbox data area.
- **Write Copy**: Write Copy mode is used when the key is required to have visibility to values on the host device but cannot change them. Values will be visible from both the virtual registry and the host device. Any writes to values are redirected to the sandbox data area. 
- **Hide**: Hide mode is used when a key on the host device could interfere with the application's ability to run properly. The application will receive a Key Not Found error code whenever an attempt is made to read from or write to values in the key even if the values exist on the host device.  

**Tip**: To apply selected isolation modes to all subkeys, right-click on the key, choose **Isolation**, select the checkbox for **Apply to Subkeys**, then **OK**.

**No Sync**

This feature only applies to virtual applications that are delivered and managed by Spoon Virtual Desktop Server. By default, keys and values in the virtual registry can be synchronized to a user's Spoon account. This enables the application state to be maintained across different devices that are Spoon enabled. If there are keys in the virtual registry that should not be synchronized and remain only on the local device, select the checkbox in the **No Sync** column next to the desired key. This setting is managed on a key level and applies to all values within that folder.

**Importing Registry Hive Files**

Spoon Studio can import registry hive (.reg) files into the virtual registry. To import a .reg file, select the **Import** button in the **Registry** panel, then choose the registry hive file to import.

### Runtimes and Components

(Coming soon)
