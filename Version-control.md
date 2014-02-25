# Using version control with history.state.gov data

All of the data on history.state.gov is stored in a version control system. The version control system ensures that every important change we make is backed up and secure, so we don't lose any work and have a complete history of our work. This article contains instructions for using our version control system on a day to day basis. It assumes you have already [set up](Setup) your system.

## Starting the day

Make sure you are always working with the latest version of every file in your system - both in your SVN Working Copy and in your eXist-db database. The best way to do this is to follow these directions:

### Getting the latest files

1. In Syncro SVN Client, select the `Working Copy` tab.
1. Right-click on the top-level folder and select `Update`. 
1. Select the `Console` tab to watch the progress as the SVN client downloads all of the latest files. 
1. When you see `Updated to revision ____` (e.g., "1000") and `Refresh done`, then the SVN client has completed downloading all of the files.
1. Your SVN Working Copy is now up to date. 

### Repopulating the database

1. Warning: This step will erase the contents of your eXist-db database. So before you proceed, make sure you have saved any files you've edited from the database into your SVN Working Copy. 
1. Stop eXist-db. You'll know eXist-db is running if the eXist-db icon in your task bar (Windows) or menu bar (Mac) and the `Start server` option is grayed out. To stop the eXist-db server, select the eXist-db task/menu bar icon > `Stop Server`. You'll know eXist-db is not running if there is no eXist icon in the task/menu bar, or if you click on the icon and the menu option for `Stop Server` is grayed out.
1. In oXygen, select the `Tools` menu > `External Tools` > `wipe-exist-data`. A new tab will open at the bottom of the oXygen window, showing the results of the `wipe-exist-data` script. When you see `BUILD SUCCESSFUL`, close the tab.
1. Start eXist-db. To start eXist-db if you already have the eXist-db icon in your task/menu bar, select `Start Server`. If you do not have the eXist-db icon in your task/menu bar, double-click the shortcut to `start.jar` that you created when you [set up](Setup) your system.

## Committing new or edited files to the repository

1. When you have a new or edited file that you are ready to commit to the repository, open the Syncro SVN Client window and select the `Working copy` tab. The SVN Client should automatically scan for new or edited files. (You can force the SVN Client to scan for new or edited files by right-clicking on the top-level folder and selecting `Refresh`.
1. Right-click on the top-level folder and select `Commit`. A `Commit` dialog window will appear, showing a list of all changed files. Carefully review the list of changed files, ensuring that the only files checked are the ones you want to commit; uncheck any files you do not want to commit. In the `Commit message` field enter a description of the changes you are making.
1. Select the `Console` tab to watch the progress as the SVN client commits your files to the repository.
1. When you see `Committed revision ____` (e.g., "1001") and `Refresh done`, then the SVN client has completed committing your files to the repository.