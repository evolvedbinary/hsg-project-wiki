# Setting up a history.state.gov development system

Setting up a history.state.gov development system requires a modern computer with ample memory and storage and a suite of software (eXist-db, oXygen XML Editor, and GitHub Desktop). These steps will lead you through installing this software, checking out GitHub repositories, setting up oXygen to access the files in the history.state.gov project, starting eXist, and populating the database. When complete, you will have a fully functional copy of the history.state.gov ("hsg") website running on your computer, and you can edit files and preview how the changes will look on your computer. Once you are ready to publish your work, commit and sync the changes into our version control system.

## Requirements

- A computer with at least:
    - 8 GB of RAM (to accommodate editing large XML files in oXygen, and running eXist, a web browser, and other apps at the same time)
    - 20 GB of available storage space (an SSD, or SSD/HD hybrid "Fusion" drive, is recommended for best performance).
- Mac OS X (up to date). The Mac operating system is not strictly necessary; on Windows, Linux, or other operating systems, you should be able to find alternative methods of installing the required software, but you're on your own.
- A GitHub account
    - If you don't have one, please create one at https://github.com/join
    - Make note of your credentials (username & password), since you will need these for the steps below.
    - Let your trainer know your GitHub account name so we can add you to the HistoryAtState organization on GitHub, also needed for the steps below.
- Other required information that will be provided to you during your training
    - Credentials for hsg (needed when you begin publishing directly to hsg)
    - oXygen license key
    - Mac App Store credentials (needed for downloading Transmit)

## Updating from an old setup? 

- Instructions last updated December 15, 2017.
- **Note:** Please contact Joe if you encounter any unexpected results as you follow these directions for updating your system.
- Open the App Store (using Spotlight, search for `App Store`; or in Finder, select `Go` > `Applications`), select the Updates tab, and install all available updates; if the App Store prompts you to restart your system to complete installation of operating system updates, do so before proceeding.
- You will need updates to the packages we install via Homebrew. To update these packages, open Terminal (using Spotlight, search for `Terminal`; or in Finder, select `Go` > `Utilities`), and paste in the following command: 

        brew update && brew upgrade

- We also need to make sure you have all of the required dependencies installed. To show what you have already installed, paste in the following command:
 
        brew list

    You should see all 4 of the following entries: `ant git node node@4`. If any are missing, install them with the following command:

        brew install ant git node node@4

- Run `brew doctor` to check your Homebrew installation, and follow any instructions to resolve problems that it reports. Keep running `brew doctor` until it reports:

    > Your system is ready to brew.

- These steps will update you to eXist 3.6.0, GitHub Desktop 1.0.9, oXygen 19.1, and Java 8 Update 152 (or newer):

        brew tap caskroom/versions
        
        brew cask uninstall java github-desktop
        
        brew cask reinstall exist-db github java8 oxygen-xml-editor

- GitHub Desktop has recently been relaunched with a significantly different interface. Proceed with the setup steps under [Setting up GitHub Desktop](#setting-up-gitHub-desktop). Return here when you are done setting up GitHub Desktop.
- Next, open oXygen.
  - From the External Tools toolbar menu (i.e., the green triangle icon), select "Fetch updates for all repositories". 
  - Quit and restart oXygen.
  - Then, from the External Tools toolbar menu, run these commands:
    - `Wipe eXist Data` (confirm "yes" when asked)
    - Steps 1-3 under `Setup and maintenance`: `Clone all repositories`, `Apply Mac settings to hsg-project`, and `Apply hsg-project settings to eXist`
- If you use eXist to preview website content, then proceed to [Starting eXist](#starting-exist) and then perform the steps under [Deploying all repositories to eXist](#deploying-all-repositories-to-exist). Return here when you are done setting up eXist.
- If you just installed the new GitHub Desktop above and had to re-authenticate, you'll need to re-add all of your repositories to the list. To do this, you can either (1) drag the repository folders from the Finder onto the GitHub Desktop window or (2) add the repository folders one at a time from oXygen by opening a file from a repository and selecting `Open current repository in GitHub Desktop` from the oXygen External Tools menu (as described in [Commit your work and push it to GitHub](#commit-your-work-and-push-it-to-github). 

## Installing other dependencies

1. Open Terminal (using Spotlight, search for `Terminal`; or in Finder, select `Go` > `Utilities`). Paste the following commands into your Terminal window, one at a time, hitting return after each:

        mkdir ~/workspace

        open ~/

    These commands create a new directory, `workspace`, in your home directory, and open your home directory in the Finder. (You could easily have created this new directory in the Finder too, but performing these steps on the command line primes you for the steps to come.)

1. We will be storing all files related to our work in this directory and accessing it a lot, so drag the new `workspace` folder from your home directory in the Finder onto that window's Favorites sidebar:

    ![Video showing how to drag folder onto sidebar](https://github.com/HistoryAtState/hsg-project/wiki/images/drag-folder-onto-sidebar.gif)
    
1. Install [Homebrew](http://brew.sh) by copying and pasting this entire command into your Terminal window and hitting return:

        /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"

    Follow any instructions that Homebrew presents you with. For example, it may prompt you to install XCode or accept the XCode license (in which case you should do as directed, and once done, re-enter the command to install homebrew).  

1. To confirm Homebrew installed correctly, enter this command:

        brew doctor

    You should see the following response:

    > Your system is ready to brew.

    If, instead, homebrew reports that there are errors or issues, try to follow the prompts to fix the problems. If you're unable to fix them, ask your trainer for help. Do not proceed until the `brew doctor` command reports you that your system is "ready to brew".

1. Having installed Homebrew, enter these commands to install Java 8 JDK, GitHub Desktop, oXygen XML Editor, eXist, as well as the other dependencies:

        brew tap caskroom/versions

        brew cask install java8 github oxygen-xml-editor exist-db

        brew install ant git node node@4

        npm install -g gulp bower 

    When prompted, enter the password for your user account. You will now find oXygen XML Editor and GitHub Desktop in the `Applications` folder of your home directory (e.g., `/Users/Joe/Applications`, not in the top level `/Applications` directory). To find the application in Finder, select `Go > Home > Applications`. Drag the oXygen XML Editor and GitHub Desktop icons into your Dock so you can always get to them easily.

1. Start oXygen and paste in the license key provided to you during your training when prompted.

1. Quit oXygen for now.

## Setting up GitHub Desktop

1. Find GitHub Desktop in your Applications folder. Drag its icon to the dock.

1. Start GitHub Desktop, and follow the prompts to set up the program: Select `Sign into GitHub.com` and enter your GitHub.com user account credentials. Select `Continue`. Under the `GitHub Desktop` menu bar, select `Install Command Line Tool`.

## Getting hsg-project

1. To clone the hsg-project repository which contains all of the files needed for working with history.state.gov, click on the `File` menu and select `Clone Repository`. Under GitHub.com, you will see an entry called `HistoryAtState/hsg-project`. Select this, and then select `Clone hsg-project`. In the `Local Path` field, click on `Choose`, navigate to your `workspace` folder we created in step 1, select `Open`, and finally select `Clone`.

1. In Finder, navigate to your `workspace` directory, and notice that `hsg-project` is saved here. Inside the `hsg-project` folder, double-click on the `hsg-project.xpr` file. This opens oXygen and opens the `Project` pane with a list of the top level folders in the `hsg-project` folder. You can use this to open any file in our project for editing.

1. In oXygen's menu bar, you will see a button with a green triangle icon, the "Tools" icon, and at its right is a small black triangle icon. Select the black triangle icon to display a list of common commands for working with the hsg-project. Under the section `Setup & maintenance`, select `1. Clone all repositories`. (These commands are also available via the oXygen menu bar: `Tools` > `External Tools`.) This triggers a command to clone all of the data repositories for hsg, including `frus` (for the _Foreign Relations_ series), `pocom` (for Principals & Chiefs), etc. These repositories will all be stored in the `repos` subdirectory of the `hsg-project` directory. On our DIN, `Clone all repositories` procedure should take only a couple of minutes; on slower internet connections, this may take 15 minutes or longer. You will know the command completed successfully when you see `BUILD SUCCESSFUL` in the console pane at the bottom of your oXygen window. 

1. In the Project pane, right-click on the `hsg-project` folder and select `Refresh`. You should now see a `repos` folder. Explore the `repos` directory and notice, for example, `frus` and `pocom`. These are all of the files that make up hsg.

1. Next, continue with the `Setup & maintenance` entries in the External Tools menu, selecting `2. Apply Mac settings to hsg-project` and `3. Apply hsg-project settings to eXist`. (Do not proceed to #4 yet.)

## Previewing files on your computer

Before publishing files to the public website, you may wish to preview them on your local system. This helps ensure that your edits have the desired effect and reduces the likelihood of unforeseen problems when publishing the files.

### Starting eXist

1. Open the `Applications` folder (in Finder, select `Go > Applications`). Drag eXist onto your Dock. 

1. Start eXist. The first time you run eXist the installer will lead you through a setup screen. Accept all of the default values, and select `Save` and `Yes`. The installer will close. 

1. Start eXist. eXist will show a splash screen and install some default apps. Once the splash screen disappears, you can start, stop, and quit eXist and access other eXist utilities via its menu bar entry (a blue "X"-shaped icon). 

### Deploying all repositories to eXist

1. To prepare your local eXist database with all of the files needed to run a local copy of hsg, go to the Tools dropdown menu in oXygen and select `4. Deploy all repositories to localhost`. This step takes about 10-15 minutes on our computers. On a remote computer, this step could take as long as 40 minutes, depending on your computer's hardware.

1. Now a complete copy of history.state.gov is now running at <http://localhost:8080/exist/apps/hsg-shell/>. 

### Test a simple edit

1. Drill down through the folder structure of the repositories to `repos/rdcr/articles/afghanistan.xml`. This is a TEI XML file that can be viewed at <http://localhost:8080/exist/apps/hsg-shell/countries/afghanistan> (see <http://history.state.gov/countries/afghanistan> for the public website).

1. Make a simple change, e.g., change the word "Summary" to "Introduction" in the first `<head>` element. Select the `File` menu > `Save` to save your changes to the file. 

1. To upload the file to eXist, select the `Tools` dropdown > `Upload current file to localhost`. A new tab will open at the bottom pane of the oXygen window, showing the results of the `Upload current file to localhost` script. When you see `BUILD SUCCESSFUL`, close the tab.

1. In your browser, (re-)load <http://localhost:8080/exist/apps/hsg-shell/countries/afghanistan>, and you will see your change. Return to oXygen, undo your change (`Edit` > `Undo`), save the file, and select `Upload current file to localhost` again.

## Publishing your work

When you have work that you would like to publish, the following steps will ensure that your work is backed up in our version control repository and published to the live site:

### Apply "Format and Indent"

With the document open in oXygen, select the "Format and Indent" toolbar button (also accessible via the "Source" menu under "Document" > "Format and Indent"), and save your document. If you've made updates to many files, see https://github.com/HistoryAtState/hsg-project/issues/31 for a time-saving tip.  This "Format and Indent" command ensures your commit (in the next step) is clean and reveals only the changes you made, not other incidental changes.

### Commit your work and push it to GitHub 

1. With a file from the repository open in oXygen, open the Tools dropdown menu, and select `Open current repository in GitHub Desktop`. This will open GitHub Desktop, with the current repository selected. 

1. You will see a button in the toolbar, called `Fetch origin`. Click on this to make sure you have the latest version of everyone else's work from this repository.

1. In the left-hand pane, you will see a tab, called `Changes`, with a list of files you have modified. Confirm which files you want to commit by selecting (or unselecting) the checkboxes next to each file. Clicking on any file bring up a summary of the changes you made to the file (known as a "diff").

1. Enter a brief summary of the changes in the `Summary` field. If the description is too long, use the `Description` field. 

1. Once you've confirmed that the files and summary/description are correct, select `Commit to master`. 

1. As soon as you make a commit, the `Fetch Origin` toolbar button will become `Push Origin`, meaning that you have at least one local commit that have not yet been pushed to GitHub's servers. 

1. Once you've made all of your commits, select `Push Origin` to push your changes to GitHub. (The `Push Origin` button will now return to `Fetch Origin`, and you will receive an email confirmation of your commit from the hsg-commits mailing list.)

### Publish your work to hsg

1. Publish the changes to hsg in oXygen using the Tools dropdown menu > `Upload current file to history.state.gov`. 

    Before you publish the first time, you must first enter the password provided to you during your training for the hsg production servers. To do this, in oXygen's Tools toolbar dropdown menu under the `Setup & maintenance section`, select `5. Enter server credentials`.

## Keeping up with everyone's latest work

To ensure your local copy of files is up to date with everyone's work, follow these steps:

1. In oXygen, under the Tools dropdown menu, select `Fetch updates for all repositories`. Or, if you are only interested in a single repository, open a file from that repository in oXygen and select `Fetch updates for current repository`. A new tab will open at the bottom pane of the oXygen window, showing the results of the update script. These results summarize which files have been downloaded. When you see `BUILD SUCCESSFUL`, feel free to close the tab.

1. To update your copy of the website running in eXist, select `Deploy all repositories to localhost`. Or, if you only want to update a single repository, open a file from that repository in oXygen and select `Deploy current repository to localhost`. When you see `BUILD SUCCESSFUL`, close the tab.

A more detailed set of instructions with suggestions for daily and weekly tasks is in the article on [[Version control]].

## Browse localhost's eXist in oXygen's Data Source Explorer

1. Note: This is optional and used for editing files already stored in the database on your local copy of eXist. The previous section provides a convenient method of uploading individual files to eXist.

1. In oXygen, select the `Window` menu > `Show View` > `Data Source Explorer`. A new pane will open up, called `Data Source Explorer`.

1. In this pane's toolbar, click on the small gear icon (its tooltip labels this icon as `Configure Database Sources...`). A `Preferences` window will appear.

1. Under `Connection wizards` select `Create eXist-db XML connection`. Keep `Host`, `Port`, and `Libraries` unchanged, but modify the other fields as follows:

    - `User:` `admin`
    - `Password:` (delete the existing entry and leave this blank)

1. Leave `Use a secure HTTPS connection (SSL)` checkbox unchecked. 

1. Select `OK`

## Browsing history.state.gov's eXist in oXygen's Data Source Explorer

Note: These steps are optional and used for editing files already stored in the database on history.state.gov. The previous section provides a convenient method of uploading individual files to eXist.

1. In oXygen, select the `Window` menu > `Show View` > `Data Source Explorer`. A new pane will open up, called `Data Source Explorer`. 

1. In this pane's toolbar, click on the small gear icon (its tooltip labels this icon as `Configure Database Sources...`). A `Preferences` window will appear. 

1. Under `Connections` select `+` to bring up the `Connection` window. Modify the fields as follows: 

    - `Name:` `1861.history.state.gov`
    - `Data Source:` `WebDAV (S)FTP`
    - `WebDAV/FTP URL:` `https://1861.history.state.gov/exist/webdav/db`
    - `User` and `Password`: You will be provided with this information during your training

1. Select `OK`

1. Add another connection using the same steps for `1991.history.state.gov`

## Connecting to hsg with Transmit

Transmit is a file transfer client that makes it easy to upload many files to eXist or perform other batch file processes. You will be provided with this software during your training.

1. Select `Favorites` > `Add to Favorites...`

1. Modify the fields as follows: 

    - Name: `1861.history.state.gov`
    - `Protocol:` `WebDAV HTTPS`
    - `Server:` `1861.history.state.gov`
    - `User Name` and `Password`: You will be provided with this information during your training
    - `Remote Path`: `/exist/webdav/db`
    - `Local Path`: `~/workspace/hsg-project/repos`

1. Select `Save`.

1. Add another favorite using the same steps for `1991.history.state.gov`

1. For localhost, use:

    - Name: `localhost hsg`
    - `Protocol:` `WebDAV`
    - `Server:` `localhost`
    - `User Name` and `Password`: You will be provided with this information during your training
    - `Remote Path`: `/exist/webdav/db`
    - `Local Path`: `~/workspace/hsg-project/repos`