While developing with node.js in Windows I occasionally run into the issue where I can't delete a node module because the module has redundant subfolders that exceed Windows path limit of 260 characters thus forcing me to traverse each subfolder only to rename them to some arbitrary one length character.

After doing some research into this issue I have came to the conclusion that the problem isn't Windows, it's the modules author and what seems like their inherent lust for deep hierarchies.

### Solution One

In order to delete the path, getting around the 260 character limit, we must use the following alternate file designation methodology:

* **Shift-right-click** on the item to be deleted and select **copy as path**
* Open a **command prompt** then type **DEL** then **paste the name of the path** you copied
* Next, go to the **beginning of the line**, and **modify** the **drive letter** for the Windows path so that it reads like <b>\\\\?\&lt;driveletter&gt;:\</b>
> For example, if the path begins with C:\ modify so it reads as \\\\?\C:\
* Windows will now delete the file or folder successfully

### Solution Two

If you're not a command line warrior here's a solution for you! The guys over at [BackupChain](http://backupchain.com) created a freeware GUI tool called [DeleteLongPath](http://backupchain.com/DeleteLongPath.html) that does exactly what is described in solution one provided your user account has the permissions to do so.

### Wrap Up

If either one of these solutions helped you out leave a comment below! I'm curious as to how many people have ran into this problem.