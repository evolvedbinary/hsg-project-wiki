# Setting up a history.state.gov development system

Setting up a history.state.gov development system requires installing oXygen XML Editor, checking out the Subversion (SVN) repository, setting up oXygen to access the files in the history.state.gov project, starting eXist-db and populating the database. Then you will have a complete copy of the history.state.gov website running on your computer, and you can edit files and preview how the changes will look on your computer. Once you have completed these steps, read the page on [version control](version-control) to learn how to save your work into our version control system and ensuring you continue to have the latest version of all files in our repository.

## Requirements

- Java 8 JDK
- oXygen XML Editor (v16.1+)
- The username and password for your history.state.gov group account on Assembla.com

## Installing Java 8 JDK

1. [Download](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) and install the Java 8 JDK (select the version for your operating system from the 1st table on this page)
1. Or, if you have installed [Homebrew](http://brew.sh) and [Caskroom](http://caskroom.io/), you can simply type this command into Terminal.app: `brew cask install java` 

## Installing oXygen XML Editor

1. [Download](http://www.oxygenxml.com/download_oxygenxml_editor.html) and install oXygen XML Editor. Assuming you're using Mac OS X Yosemite (10.10), the recommended version is the one labeled, "OS X 10.8 and later (Includes Java SE 8u__)" (**not** the one labeled, "Mac OS X 10.6 and later"). Direct link: http://mirror.oxygenxml.com//InstData/Editor/MacOSX/VM/oxygen.tar.gz.

1. Start oXygen and paste in license key when prompted

## Checking out the Subversion (SVN) repository

1. Start Syncro SVN Client
1. Select the Repositories tab
1. In the Repositories window select `Add a new repository` or, alternatively, select the `Repository` menu > `New Repository Location`
1. In the Add SVN Repository dialog, enter `https://subversion.assembla.com/svn/paho/trunk/`, and keep `Validate repository connection` checked
1. When prompted, enter your Assembla.com username and password
1. The new repository address will appear in the list of repositories. With the repository address selected, select `Repository` > `Check out...`
1. In the `Check out` dialog, enter the location where you would like to store your local copy of the repository. Suggested values:
    - Mac: `/Users/YOUR_USERNAME/workspace/paho-trunk`
    - Windows: `C:\Users\YOUR_USERNAME\workspace\paho-trunk`
1. Keep all other options unchanged. Select `OK`
1. The check out process will take 20-60 minutes depending on your bandwidth. Select the `Console` tab to watch the progress as the SVN Client downloads all of the files. When complete, the `Console` tab will show the message, `Operation successful`.
1. The files stored in the `paho-trunk` folder are called your `SVN Working Copy`, and we will use these to set up oXygen and eXist-db on your system. Later, you will upload your edited files from the `SVN Working Copy` to the SVN repository.

## Open the history.state.gov project in oXygen

1. On your desktop navigate to the newly created `paho-trunk` folder
1. On Mac, double-click on the `project-unix.xpr` file. On Windows, double-click on the `project-windows.xpr` file. 
1. This opens oXygen and opens the `Project` view with a list of the top level folders in the `paho-trunk` project. You can use this to open any file in our project for editing. 

## Starting eXist-db and populating the database

1. In oXygen, select the `Tools` menu > `External Tools` > `prepare-for-development`
1. A new tab will open at the bottom of the oXygen window, showing the results of the `prepare-for-development`. When you see `BUILD SUCCESSFUL`, close the tab.
1. On your desktop navigate to the `paho-trunk` folder and open the `exist` subfolder. Double-click the `start.jar` file to start eXist-db. Wait for the eXist-db splash screen to appear, report "started," and disappear. Also, the eXist-db icon will appear in your task bar (Windows) or menu bar (Mac). Note: This task/menu bar entry lets you `Start` and `Stop` the server; the `Quit` option stops the server and ends the task/menu bar entry.
1. Since you will use `start.jar` every time you first start eXist-db up, you may wish to create a shortcut to this file.
    - On Windows, right-click on `start.jar` and select `Create Shortcut`. Move the shortcut file to a convenient location (e.g., your desktop)
    - On Mac, drag `start.jar` to the Dock beside the trashcan icon.
1. In oXygen, select the `Tools` menu > `External Tools` > `populate:all`. This will upload the entire contents of the SVN working copy (in `paho-trunk/db/`) to eXist. This can take 7-20 minutes depending on the speed of your computer. When you see `BUILD SUCCESSFUL`, close the tab.
1. Now you have a complete copy of the history.state.gov server running on your computer, accessible at <http://localhost:8080/>

## Editing files and previewing them in eXist-db

1. To open a file, use the Project window to drill down through the working copy's folder structure. For example, drill into `db/cms/apps/tei-content/data/countries/afghanistan.xml`. This is a TEI XML file that can be viewed at <http://localhost:8080/countries/afghanistan> (see <http://history.state.gov/countries/afghanistan> for the public website).
1. Make a simple change, e.g., change the word "Summary" to "Introduction" in the first `<head>` element. Select the `File` menu > `Save` to save the file. 
1. To upload the file to eXist-db, select the `Tools` menu > `External Tools` > `upload-current-file-to-localhost`. A new tab will open at the bottom of the oXygen window, showing the results of the `upload-current-file-to-localhost` script. When you see `BUILD SUCCESSFUL`, close the tab.
1. In your browser, (re-)load <http://localhost:8080/countries/afghanistan>, and you will see your change. Return to oXygen, undo your change (`Edit` > `Undo`), save the file, and select `upload-current-file-to-localhost` again.

## Setting up oXygen's Data Source Explorer

1. Note: This is optional and used for opening files directly from the database. The previous section provides a convenient method of uploading individual files to eXist-db.
1. In oXygen, select the `Window` menu > `Show View` > `Data Source Explorer`. A new pane will open up, called `Data Source Explorer`. 
1. In this pane's toolbar, click on the small gear icon (its tooltip labels this icon as `Configure Database Sources...`). A `Preferences` window will appear. 
1. Under `Connection wizards` select `Create eXist-db XML connection`. Keep `Host`, `Port`, and `Libraries` unchanged, but modify the other fields as follows: 
    - `User:` `admin`
    - `Password:` (delete the existing entry and leave this blank)
    - `eXist Admin Client JWS:` delete `exist/` so that the entry reads: `webstart/exist.jnlp`
1. Leave `Use a secure HTTPS connection (SSL)` checkbox unchecked. 
1. Select `OK`