# Update your history.state.gov Development Environment

> Last updated on December 31, 2019 for eXist 5.1.1.

These directions assume you have a [fully configured](setup) history.state.gov Development Environment and that you need to update your system so that it contains the latest operating system, software dependencies, core applications, and configurations.

## Install system updates

1. From the Apple () menu in the top-left corner of your screen, choose "About This Mac". If it says "macOS Catalina Version 10.15.x", click on the "Software Update" button and install any available software updates. If you are running an earlier version of macOS, here is the link to the [macOS Catalina webpage](https://apps.apple.com/us/app/macos-catalina/id1466841314?mt=12); go to this page and click on the "View in the Mac App Store" button to open Catalina in the App Store. Click on the Download button, and allow the installation to complete. 
    - If your computer tells you that it cannot run macOS Catalina (10.15), worry not; all of our software is still compatible with systems as old as macOS High Sierra (10.13), but the directions below assume that you have Catalina.

1. Open the App Store (using the Spotlight (🔍) icon in the menu bar, search for `App Store`; or in Finder, select `Go` > `Applications`, and find `App Store` in the list of applications), select the Updates tab, and install all available updates. 
    - _Note_: If you receive an update alert for Transmit, you will need to delete the local version you have installed and follow these steps instead: [Connect to hsg with Transmit](setup#connect-to-hsg-with-transmit)

## Update the Apple Developer Command Line Tools

1. After installing all system updates, you will need to install the current version of the Apple Developer Command Line Tools. To do so, open Terminal (using Spotlight, search for `Terminal`; or in Finder, select `Go` > `Utilities`), and paste in the following command:

        xcode-select --install

1. If this command returns a message that says, "error: command line tools already installed", then you can skip to the next step. Otherwise, you will see a dialog window with 3 buttons. Select "Install", and let the installation complete. Or, it might tell you that you already have these tools installed, in which case you should proceed.

## Update Homebrew

1. We need to update Homebrew. To do so, enter the following command: 

        brew update && brew upgrade

## Update HSG software dependencies

1. With Homebrew updated, we will use it to make sure you have all of the HSG software dependencies installed. To show what you have already installed, paste in the following command:
 
        brew list

1. You should see at least the following 4 entries: `ant git node node@10`. If you see `node@4`, `node@6`, and/or `node@8`, these are old versions, and we should uninstall these. Paste in the following command(s) to uninstall the old versions (4 through 8), as appropriate:

        brew uninstall node@4

        brew uninstall node@6

        brew uninstall node@8

    If any of the other 4 required dependencies—`ant`, `git`, `node`, or `node@10`—are missing from the result of the `brew list` command above, install them with the following commands, as needed:

        brew install ant

        brew install git

        brew install node

        brew install node@10

1. We will now install one new dependency, `maven`:

        brew install maven

1. Run `brew doctor` to check your Homebrew installation, and follow any instructions to resolve problems that it reports. Keep running `brew doctor` until it reports:

    > Your system is ready to brew.

    Sometimes the problems reported by `brew doctor` are inscrutable. Contact Joe if you are unable to decipher these.

1. If you just upgraded from node@6 to node@10 in the step above, enter the following three commands to update node-related dependencies.

        npm install -g gulp bower

        cd ~/workspace/hsg-project/repos

        for folder in $(find * -maxdepth 0 -type d ); do rm -rf "$folder/node_modules"; done

## Update HSG applications

1. We also need to make sure you have the rest of the HSG applications installed. To show what you have already installed, paste in the following command:
 
        brew cask list

    You will probably see the following 4 entries: exist-db, github, java, and oxygen-xml-editor. However, if you see `java8`, `java11`, or `github-desktop`, paste in the corresponding command(s) to uninstall the old software, as appropriate:

        brew cask uninstall java8

        brew cask uninstall java11

        brew cask uninstall github-desktop

1. If eXist, GitHub Desktop, or oXygen are open, quit these applications.

1. The next commands will update you to the current versions of our main software packages: Java (OpenJDK 13.0.1), eXist 5.1.0, GitHub Desktop 2.2.4, and oXygen XML Editor 21.1:

        brew cask reinstall java

        brew cask reinstall exist-db 

        brew cask reinstall github 

        brew cask reinstall oxygen-xml-editor

    > If the command for java returns an error like `Error: Cask 'java' is unreadable: undefined method `release' for OS::Mac:Module`, then follow the [directions for completely removing old versions of Java](troubleshooting#remove-all-versions-of-java), and return here when you are done.
    >
    > If any of the other commands return an error like `Error: It seems there is already an App at '/Applications/GitHub Desktop.app'`, go to the Finder, select `Go` > `Applications`, locate the application in question, drag its icon to the Trash icon in the Dock, and perform the Terminal command one more time. 

## Prepare oXygen and eXist

1. Next, open oXygen.
    - From the External Tools toolbar menu (i.e., the green triangle icon), select `Fetch updates for all repositories`. 
    - Quit and restart oXygen.
    - Then, from the External Tools toolbar menu, run the `Wipe eXist Data` command (confirm "yes" when asked)

1. Next, we need to force eXist to complete one full start up, in order to work around some quirks of eXist and macOS:
    - Click on eXist's dock icon.
    - A dialog box will open asking if you want to open eXist. Select `Open`.
    - eXist's dock icon will stop bouncing.
    - Click on eXist's dock icon again.
    - A dialog box will open showing eXist's configuration properties. Select `Save`. When prompted to create the data directory or confirm the location of the data directory, select `OK`.
    - The eXist splash screen will appear as eXist completes its startup routine.
    - Once the eXist splash screen disappears, click on the eXist menu bar icon and select `Quit`.

1. Return to oXygen.
    - From the External Tools toolbar menu, under `= Setup and maintenance =`, select
        - `1. Clone all repositories & resources`
        - `2. Apply Mac settings to hsg-project`
        - `3. Apply hsg-project settings to eXist`

1. If you use eXist to preview website content, then proceed to [Start eXist](setup#start-exist) and then perform the steps under [Deploy all repositories to eXist](setup#deploy-all-repositories-to-exist). Otherwise, you're all set with the latest version of all of our software.
