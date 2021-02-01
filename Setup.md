# Set up a history.state.gov Development Environment

> Last updated on January 28, 2021. (**Note:** To update an existing HSG Development Environment, use [these directions](update).)

The history.state.gov (HSG) Development Environment requires a modern computer with ample memory and storage and a suite of software. These directions lead you through installing this software, checking out GitHub repositories, and setting up oXygen to access the files in the history.state.gov project. If you need to preview how files will look on the history.state.gov website before you publish them, there are further directions for starting eXist and populating the database, at which point you will have a fully functional copy of the website running on your computer. The directions also describe how to save your work by committing it to our version control system and, once you are ready to publish it, how to upload your work to the website.

## Requirements

- A computer with at least:
    - 8 GB of RAM (to accommodate editing large XML files in oXygen, and running eXist, a web browser, and other apps at the same time)
    - 50 GB of available storage space (an SSD is recommended for best performance).
- An up-to-date Mac, running macOS 11 or higher. While macOS is not strictly necessary, we provide our own support, so you'll be on your own in finding methods for installing the required software and adapting scripts, etc. to Windows or Linux.
- A GitHub account
    - If you don't have one, please create one at https://github.com/join.
    - Make note of your credentials (username & password), since you will need these for the steps below.
    - Let your trainer know your GitHub account name so we can add you to the HistoryAtState organization on GitHub.
- Other required information that will be provided to you during your training (see the [training checklist](training-checklist))

## Install system updates

1. From the Apple () menu in the top-left corner of your screen, choose `About This Mac`. Select the `Software Update` button and install any available software updates. If you are running a version of macOS lower than 11 (Big Sur), here is the link to the [macOS Big Sur webpage](https://support.apple.com/en-us/HT201475) listing requirements and instructions for upgrading. 
    - If your computer cannot run macOS Big Sur (11), worry not; all of our software is still compatible with systems as old as macOS Mojave (10.14), but the directions below assume that you have Big Sur.

1. Open the App Store (using the Spotlight (🔍) icon in the menu bar, search for `App Store`; or in Finder, select `Go` > `Applications`, and find `App Store` in the list of applications), select the Updates tab, and install all available updates. 

## Prepare your workspace

1. Open Terminal (using the Spotlight (🔍) icon in the menu bar, search for `Terminal`; or in Finder, select `Go` > `Utilities`). Paste the following commands into your Terminal window, one at a time, hitting return after each:

        mkdir ~/workspace

        open ~/

    These commands create a new directory, `workspace`, in your home directory, and open your home directory in the Finder. (You could easily have created this new directory in the Finder too, but performing these steps on the command line primes you for the steps to come.)

1. We will be storing all files related to our work in this directory and accessing it a lot, so drag the new `workspace` folder from your home directory in the Finder onto that window's Favorites sidebar:

    ![Video showing how to drag folder onto sidebar](https://github.com/HistoryAtState/hsg-project/wiki/images/drag-folder-onto-sidebar.gif)
    
## Install Homebrew

1. Install [Homebrew](https://brew.sh) by copying and pasting this entire command into your Terminal window and hitting return:

        /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

    Follow any instructions that Homebrew presents you with. For example, it may prompt you to install Xcode or accept the Xcode license (in which case you should do as directed, and once done, re-enter the command to install Homebrew).  

1. To confirm that Homebrew installed correctly, enter this command:

        brew doctor

    You should see the following response:

    > Your system is ready to brew.

    If, instead, Homebrew reports that there are errors or issues, try to follow the prompts to fix the problems. If you're unable to fix them, ask your trainer for help. Do not proceed until the `brew doctor` command reports you that your system is `ready to brew`.

## Install HSG software dependencies and applications

1. Having installed Homebrew, enter these commands to install eXist, oXygen XML Editor, GitHub Desktop, OpenVPN Connect, as well as the other dependencies:

        brew tap bell-sw/liberica
        
        brew install liberica-jdk8-full exist-db github openvpn-connect oxygen-xml-editor

        brew install ant git maven node node@10

        npm install -g gulp bower

    When prompted, enter the password for your user account. 

1. eXist-db, oXygen XML Editor, GitHub Desktop, and OpenVPN Connect are now installed! To find them, go to Finder, and select `Go > Applications`. 

1. Drag the eXist-db, oXygen XML Editor, GitHub Desktop, and OpenVPN Connect icons into your Dock so you can always get to them easily. (oXygen's is inside a folder of the same name.)

1. We need to force eXist to complete one full start up, in order to work around some quirks of eXist and macOS:
    - Find eXist-db in the Applications folder.
    - Right-click (or hold down the control key and left-click) on it, and select `Open` from the contextual menu.
    - A dialog box will open asking if you want to open eXist. Select `Open`.
    - eXist's dock icon will start bouncing, then stop.
    - Click on eXist's dock icon again.
    - A dialog box will open showing eXist's configuration properties. Select `Save`. When prompted to create the data directory or confirm the location of the data directory, select `OK`.
    - The eXist splash screen will appear as eXist completes its startup routine.
    - Once the eXist splash screen disappears, click the eXist menu bar icon and select `Quit`.

## Prepare oXygen

1. Start oXygen and paste in the license key provided to you [during your training](training-checklist) when prompted.

1. Quit oXygen for now.

## Set up GitHub Desktop

1. Find GitHub Desktop in your Applications folder. Drag its icon to the dock.

1. Start GitHub Desktop, and follow the prompts to set up the program: Select `Sign into GitHub.com` and enter your GitHub.com user account credentials. Select `Continue`.

## Get hsg-project

1. To clone the hsg-project repository which contains all of the files needed for working with history.state.gov, click on the `File` menu and select `Clone Repository`. Under GitHub.com, you will see an entry called `HistoryAtState/hsg-project`. Select this, and then select `Clone`. In the `Local Path` field, click on `Choose`, navigate to your `workspace` folder we created in step 1, select `Open`, and finally select `Clone`.

1. In Finder, navigate to your `workspace` directory, and notice that `hsg-project` is saved here. Inside the `hsg-project` folder, double-click on the `hsg-project.xpr` file. This will cause oXygen to open and activate the `Project` pane, showing a list of the top level folders in the `hsg-project` folder. You can use this to open any file in our project for editing.

1. In oXygen's menu bar, you will see a button with a green, triangle-shaped icon, the "External Tools" icon, and at its right is a tiny, black, triangle-shaped icon (a "disclosure" icon, which indicates that clicking on it will display drop-down menu). Select this disclosure icon to display a drop-down list of common commands for working with hsg-project. Under the section `Setup & maintenance`, select `1. Clone all repositories & resources`. (These commands are also available via the oXygen menu bar: `Tools` > `External Tools`.) This triggers a command to clone all of the data repositories for HSG, including `frus` (for the _Foreign Relations_ series), `pocom` (for Principals & Chiefs), etc. These repositories will all be stored in the `repos` subdirectory of the `hsg-project` directory. On our DIN, `Clone all repositories & resources` procedure should take only a couple of minutes; on slower internet connections, this may take 15 minutes or longer. You will know the command completed successfully when you see `BUILD SUCCESSFUL` in the console pane at the bottom of your oXygen window. 

1. In the Project pane, right-click on the `hsg-project` folder and select `Refresh`. You should now see a `repos` folder. Explore the `repos` directory and notice, for example, `frus` and `pocom`. These are all of the files that make up HSG.

1. Next, continue with the `Setup & maintenance` entries in the External Tools menu, selecting `2. Apply Mac settings to hsg-project` and `3. Apply hsg-project settings to eXist`. (Do not proceed to #4 yet.)

## Preview files on your computer

Before publishing files to the public website, you may wish to preview them on your local system. This helps ensure that your edits have the desired effect and reduces the likelihood of unforeseen problems when publishing the files.

### Start eXist

1. Start eXist via its application icon in your Dock. During startup eXist will show a splash screen. Once the splash screen disappears, you can start, stop, and quit eXist and access other eXist utilities via its menu bar entry (a blue "X"-shaped icon). 

### Deploy all repositories to eXist

1. To prepare your local eXist database with all of the files needed to run a local copy of HSG, go to the Tools dropdown menu in oXygen and select `4. Deploy all repositories to localhost`. This step takes about 10-15 minutes on our computers. On a remote computer, this step could take as long as 40 minutes, depending on your computer's hardware.

1. Now a complete copy of history.state.gov is now running at <http://localhost:8080/exist/apps/hsg-shell/>. 

### Test a simple edit

1. Drill down through the folder structure of the repositories to `repos/rdcr/articles/afghanistan.xml`. This is a TEI XML file that can be viewed at <http://localhost:8080/exist/apps/hsg-shell/countries/afghanistan> (see <https://history.state.gov/countries/afghanistan> for the public website).

1. Make a simple change, e.g., change the word "Summary" to "Introduction" in the first `<head>` element. Select the `File` menu > `Save` to save your changes to the file. 

1. To upload the file to eXist, select the `Tools` dropdown > `Upload current file to localhost`. A new tab will open at the bottom pane of the oXygen window, showing the results of the `Upload current file to localhost` script. When you see `BUILD SUCCESSFUL`, close the tab.

1. In your browser, (re-)load <http://localhost:8080/exist/apps/hsg-shell/countries/afghanistan>, and you will see your change. Return to oXygen, undo your change (`Edit` > `Undo`), save the file, and select `Upload current file to localhost` again.

## Publish your work

When you have work that you would like to publish, the following steps will ensure that your work is backed up in our version control repository and published to the live site:

### Apply "Format and Indent"

With the document open in oXygen, select the `Format and Indent` toolbar button (also accessible via the `Source` menu under `Document` > `Format and Indent`), and save your document. If you've made updates to many files, see https://github.com/HistoryAtState/hsg-project/issues/31 for a time-saving tip. This `Format and Indent` command ensures your commit (in the next step) is clean and reveals only the changes you made, not other incidental changes.

### Commit your work and push it to GitHub 

1. With a file from the repository open in oXygen, open the Tools dropdown menu, and select `Open current repository in GitHub Desktop`. This will open GitHub Desktop, with the current repository selected. 

1. You will see a button in the toolbar, called `Fetch origin` (sometimes `Pull origin`). Click on this to make sure you have the latest version of everyone else's work from this repository.

1. In the left-hand pane, you will see a tab, called `Changes`, with a list of files you have modified. Confirm which files you want to commit by selecting (or unselecting) the checkboxes next to each file. Clicking on any file bring up a summary of the changes you made to the file (known as a "diff").

1. Enter a brief summary of the changes in the `Summary` field. If the description is too long, use the `Description` field. 

1. Once you've confirmed that the files and summary/description are correct, select `Commit to master`. 

1. As soon as you make a commit, the `Fetch Origin` toolbar button will become `Push Origin`, meaning that you have at least one local commit that have not yet been pushed to GitHub's servers. 

1. Once you've made all of your commits, select `Push Origin` to push your changes to GitHub. (The `Push Origin` button will now return to `Fetch Origin`, and you will receive an email confirmation of your commit from the hsg-commits mailing list.)

### Configure VPN

Before you can publish your work to HSG the first time, you need to configure OpenVPN Connect to connect to the HSG VPN.

1. Start OpenVPN Connect via its application icon in your Dock.

1. Select the `+` icon to enter the `Import File` dialog.

1. Select the `File` tab.

1. Select the `Browse` button and navigate to the .ovpn file that you were provided [during your training](training-checklist).

1. Select the `Add` button at the top-right corner of the window.

1. Toggle the connect/disconnect button to confirm you are able to connect. Try opening <http://1861.hsg> in your browser.

### Connect to the VPN

1. Start OpenVPN Connect via its application icon in your Dock.

1. Select the connect button.

### Publish your work to HSG

1. Publish the changes to HSG in oXygen using the Tools dropdown menu > `Upload current file to history.state.gov`. 

    Before you publish the first time, you must first enter the password provided to you [during your training](training-checklist) for the HSG production servers. To do this, in oXygen's External Tools toolbar dropdown menu under the `Setup & maintenance section`, select `5. Enter server credentials`.

## Keep up with everyone's latest work

To ensure your local copy of files is up to date with everyone's work, follow these steps:

1. In oXygen, under the Tools dropdown menu, select `Pull updates from all repositories`. Or, if you are only interested in a single repository, open a file from that repository in oXygen and select `Pull updates from current repository`. A new tab will open at the bottom pane of the oXygen window, showing the results of the update script. These results summarize which files have been downloaded. When you see `BUILD SUCCESSFUL`, feel free to close the tab.

1. To update your copy of the website running in eXist, select `Deploy all repositories to localhost`. Or, if you only want to update a single repository, open a file from that repository in oXygen and select `Deploy current repository to localhost`. When you see `BUILD SUCCESSFUL`, close the tab.

A more detailed set of instructions with suggestions for daily and weekly tasks is in the article on [[Version control]].

## Browse localhost's eXist in oXygen's Data Source Explorer

1. Note: This is optional and used for editing files already stored in the database on your local copy of eXist. The previous section provides a convenient method of uploading individual files to eXist.

1. In oXygen, select the `Window` menu > `Show View` > `Data Source Explorer`. A new pane will open up, called `Data Source Explorer`.

1. In this pane's toolbar, click on the small gear icon (its tooltip labels this icon as `Configure Database Sources...`). A `Preferences` window will appear.

1. Under `Connections` select `+` to bring up the `Connection` window. Modify the fields as follows: 

    - `Name:` `localhost`
    - `Data Source:` `WebDAV (S)FTP`
    - `WebDAV/FTP URL:` `http://localhost:8080/exist/webdav/db`
    - `User`: `admin`
    - `Password`: (blank)

1. Select `OK`

## Browse history.state.gov's eXist in oXygen's Data Source Explorer

Note: These steps are optional and used for editing files already stored in the database on history.state.gov. The previous section provides a convenient method of uploading individual files to eXist.

1. In oXygen, select the `Window` menu > `Show View` > `Data Source Explorer`. A new pane will open up, called `Data Source Explorer`. 

1. In this pane's toolbar, click on the small gear icon (its tooltip labels this icon as `Configure Database Sources...`). A `Preferences` window will appear. 

1. Under `Connections` select `+` to bring up the `Connection` window. Modify the fields as follows: 

    - `Name:` `1861.hsg`
    - `Data Source:` `WebDAV (S)FTP`
    - `WebDAV/FTP URL:` `http://1861.hsg:8080/exist/webdav/db`
    - `User` and `Password`: You will be provided with this information [during your training](training-checklist)

1. Select `OK`

## Connect to HSG with Transmit

Transmit is a file transfer client that makes it easy to upload many files to eXist or perform other batch file processes. You will be provided with this software [during your training](training-checklist). 

1. Select `Favorites` > `Add to Favorites...`

1. Modify the fields as follows: 

    - Name: `1861.hsg`
    - `Protocol:` `WebDAV`
    - `Address:` `1861.hsg`
    - `Port:` `8080`
    - `User Name` and `Password`: You will be provided with this information during your training
    - `Remote Path`: `/exist/webdav/db`
    - `Local Path`: `~/workspace/hsg-project/repos`

1. Select `Save`.

1. For localhost, use:

    - Name: `localhost hsg`
    - `Protocol:` `WebDAV`
    - `Address:` `localhost`
    - `Port:` `8080`
    - `User Name` and `Password`: You will be provided with this information during your training
    - `Remote Path`: `/exist/webdav/db`
    - `Local Path`: `~/workspace/hsg-project/repos`

### Updating From An Old Setup

Looking for the directions for updating from an old setup? These directions have moved to their own page: [Update your history.state.gov Development Environment](update).