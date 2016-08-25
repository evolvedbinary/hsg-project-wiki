# Using version control with history.state.gov data

All of the data on history.state.gov is stored in a version control system. The version control system ensures that every important change we make is backed up and secure, so we don't lose any work and have a complete history of our work. This article contains instructions for using our version control system on a day to day basis. It assumes you have already [set up](setup) your system.

## Starting the day

Make sure you are always working with the latest version of every file in your system - both in your hsg-project folder and in your local eXist-db database. The best way to do this is to follow these directions:

### Getting the latest files

1. In oXygen's External Tools dropdown menu (also available from the `Tools` menu > `External Tools`), select `Fetch updates for all repositories`.
1. Your hsg-project folder and all of the repositories cloned into its `repos` subdirectory is now up to date. 

### Uploading the latest files into your eXist-db

Assuming that your hsg-project folder has newer versions of files than are in your eXist-db database, you may wish to upload the new files to your database.

To get updated files into your eXist-db database, you have two choices:

1. Upload them one at a time (opening each file in oXygen and using the `Upload current file to localhost` button.
1. Repopulate the entire database. This takes longer (see the directions below) but has the advantage of ensuring all files are up to date. 

A good rule of thumb is to repopulate the entire database at the start of each work week, and on subsequent work days only upload individual files as needed.

#### Repopulating the database

1. Warning: This step will erase the contents of your eXist-db database. So before you proceed, make sure you have saved any files you've edited from the database into your SVN Working Copy. 
1. Stop eXist-db. You'll know eXist-db is running if the eXist-db icon in your task bar (Windows) or menu bar (Mac) and the `Start server` option is grayed out. To stop the eXist-db server, select the eXist-db task/menu bar icon > `Stop Server`. You'll know eXist-db is not running if there is no eXist icon in the task/menu bar, or if you click on the icon and the menu option for `Stop Server` is grayed out.
1. In oXygen's External Tools dropdown menu, select `Wipe Exist Data`. A new tab will open at the bottom of the oXygen window, showing the results of this operation. When you see `BUILD SUCCESSFUL`, close the tab.
1. Start eXist-db.
1. In oXygen's External Tools dropdown menu, select `Deploy all repositories to localhost`. A new tab will open at the bottom of the oXygen window, showing the results of this operation, which can take up to 15 minutes to complete. When you see `BUILD SUCCESSFUL`, close the tab.
1. Your eXist-db database is now up to date with the latest files from your hsg-project folder.

## Committing new or edited files to the repository

1. When you have a new or edited file that you are ready to commit to the repository, open GitHub Desktop, and select the repository in question; if the repository is not listed, return to oXygen, and in oXygen's External Tools dropdown menu, select `Open current repository in GitHub Desktop`. With the repository selected, GitHub Desktop's `Uncommitted Changes` tab will show a list of changed files. Make sure the checkbox is selected next to each file you intend to commit.
1. Enter a description of your work, and select the `Commit` button, using the larger box beneath that for extended comments. You can make more than one commit at once, grouping them together logically. When your commits are complete, select the `Sync` button. If you do not select the `Sync` button, your commits will not be sent to GitHub.