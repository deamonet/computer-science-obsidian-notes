
# How to Create Shared Folder in Windows Sandbox – Map Folder

By [Bashkarla](https://windowsloop.com/author/staff/ "View all posts by Bashkarla") / [How To](https://windowsloop.com/category/how-to/)

With configuration files, you can create shared folder between Windows Sandbox and host OS. Here’re the steps to map folder to Windows Sandbox in Windows 10.

One of the best things in the latest Windows 10 update is the Windows Sandbox. The sandbox allows you to launch full Windows in a virtual environment so that you can test untrusted software or settings before applying them to the actual system. When you close the sandbox, all changes are lost. This keeps the system safe and secure from malicious software and unwanted changes. Compared to alternatives like VMware or VirtualBox, Windows Sandbox is lightweight and easy to use.

You can [enable Windows Sandbox](https://windowsloop.com/enable-windows-sandbox-in-home-edition/) from the Windows Features panel. Once enabled, launch it from the Start menu and you are good to go. There is no need for any complicated setup. That being said, being lightweight means a compromise in terms of features. To deal with this Microsoft introduced the **Windows Sandbox configuration file**. Using the configuration files, you can do a lot of things like adding shared folders, virtual graphics, networking, startup scripts, etc.

For instance, to share files between host and Windows Sandbox, you can **create a shared folder in Windows Sandbox**. The shared folder gives you greater control over how and which files or data to share.

In this quick guide, let me show the procedure to share a folder with Windows Sandbox in Windows 10.

## Windows Sandbox Shared Folder

These are the steps to follow to create shared folder between Windows Sandbox and Windows 10.

1.  Open **File Explorer** with “Windows Key + E” keyboard shortcut.
2.  **Go to** C drive.
3.  Right-click and select “**New → Folder**” to create a folder.
4.  Type “**WS Config Files**” as the folder name.  
    ![Create-folder-to-save-windows-sandbox-config-file-070720](media/Create-folder-to-save-windows-sandbox-config-file-070720.webp)
5.  **Open** the folder you just created.
6.  Right-click and select “**New → Text Document**” to create a text file.
7.  Rename the file to “**MappedFolders.wsb**“. You can name the file anything you want but it is important that you replace .txt extension with .wsb.  
    ![Windows-sandbox-config-file-070720](media/Windows-sandbox-config-file-070720.webp)
8.  Open the WSB file with Notepad. **Right-click** on the file and select “**Open with → Notepad**“.  
    ![Open-config-file-notepad-070720](media/Open-config-file-notepad-070720.webp)
9.  **Copy the below code** and paste it in the Notepad. Replace the dummy folder path with the actual path of the folder you want to share or map.
    
    <Configuration>  
      <MappedFolders>  
        <MappedFolder>  
          <HostFolder>C:\Path\to\Folder</HostFolder>  
          <ReadOnly>false</ReadOnly>  
        </MappedFolder>  
      </MappedFolders>  
    </Configuration>
    
10.  If you want the shared folder to be **read-only**, change the value from `false` to `true` on line 5.  
    ![Code-to-add-shared-folder-in-windows-sandbox-070720](media/Code-to-add-shared-folder-in-windows-sandbox-070720.webp)
11.  **Save** the file with the “Ctrl + S” keyboard shortcut.
12.  **Close** the Notepad.
13.  Now, **double-click** on the “MappedFolders.wsb” file to launch Windows Sandbox.  
    ![Windows-sandbox-config-file-070720](media/Windows-sandbox-config-file-070720.webp)
14.  After launching the sandbox, you will see the shared folder directly on the **Desktop**.  
    ![Shared-folder-created-windows-sandbox-070720](media/Shared-folder-created-windows-sandbox-070720.webp)
15.  You can open it like any other folder.  
    ![Open-mapped-folder-in-windows-sandbox-070720](media/Open-mapped-folder-in-windows-sandbox-070720.webp)

Unlike regular Windows, Windows Sandbox mounts the shared folders directly on the desktop. That is the reason why you see the shared folder on the desktop rather than in the Network panel in File Explorer.

You can map multiple folders in Windows Sandbox. All you have to do is duplicate the <MappedFolder> tab in the above code.  This is how it looks like when you add multiple shared folders in Windows Sandbox.

<Configuration>  
  <MappedFolders>  
    <MappedFolder>  
      <HostFolder>C:\Path\to\Folder1</HostFolder>  
      <ReadOnly>false</ReadOnly>  
    </MappedFolder>  
    <MappedFolder>  
      <HostFolder>C:\Path\to\Folder2</HostFolder>  
      <ReadOnly>true</ReadOnly>  
    </MappedFolder>  
    <MappedFolder>  
      <HostFolder>C:\Path\to\Folder3</HostFolder>  
      <ReadOnly>false</ReadOnly>  
    </MappedFolder>  
  </MappedFolders>  
</Configuration>

As you can see, all we did was duplicate the MappedFolder tag in the code. You can also set the read-only mode for each mapped folder individually. Alternatively, you can also create multiple configuration files for each shared folder.

**Important note:** To access the shared folders in Windows Sandbox, you have to launch it using the configuration file. If you launch Windows Sandbox directly from the Start menu, you will not see the shared folders.

## FAQ: Windows Sandbox Mapped Shared Folders

**Can I share folders with Windows Sandbox?**

Yes. You can share any Windows folder with Windows Sandbox using the configuration files. You can find the steps to create Windows Sandbox Mapped Folder configuration file above.

**How to protect Windows Sandbox shared folder?**

To protect host folders, you can set the shared folders in read-only mode. When in read-only mode, Windows Sandbox cannot write or change data in the shared folder.

**Can I share multiple folders with Windows Sandbox?**

Yes. Using the same configuration file, you can map multiple folders. Instructions on how you can do it can be found above.

I hope that helps. If you are stuck or need some help, comment below and I will try to help as much as possible.

[![Windows 10 sandbox](media/Windows_10_sandbox.jpg)](https://windowsloop.com/how-to-transfer-files-to-windows-sandbox/ "How To Transfer Files to Windows Sandbox")

#### [How To Transfer Files to Windows Sandbox](https://windowsloop.com/how-to-transfer-files-to-windows-sandbox/ "How To Transfer Files to Windows Sandbox")

[![Windows 10 sandbox](media/Windows_10_sandbox.jpg)](https://windowsloop.com/enable-windows-sandbox-in-home-edition/ "Enable Windows Sandbox in Windows 10/11 Home Edition")

#### [Enable Windows Sandbox in Windows 10/11 Home Edition](https://windowsloop.com/enable-windows-sandbox-in-home-edition/ "Enable Windows Sandbox in Windows 10/11 Home Edition")

[![Windows 10 sandbox](media/Windows_10_sandbox.jpg)](https://windowsloop.com/enable-or-disable-sandbox-on-windows-10/ "How to Enable or Disable Sandbox on Windows 10")

#### [How to Enable or Disable Sandbox on Windows 10](https://windowsloop.com/enable-or-disable-sandbox-on-windows-10/ "How to Enable or Disable Sandbox on Windows 10")

Please signup for the WindowsLoop newsletter by clicking the following link: [WindowsLoop Newsletter Signup](https://landing.mailerlite.com/webforms/landing/q3u1e4).

-   [About](https://windowsloop.com/about-us/)
-   [Contact](https://windowsloop.com/contact/)
-   [Careers](https://windowsloop.com/careers/)
-   [Cookie Policy](https://windowsloop.com/cookie-policy/)
-   [Privacy Policy](https://windowsloop.com/privacy-policy/)

Copyright © 2023 WindowsLoop