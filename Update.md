# Update your history.state.gov Development Environment

> Last updated on January 28, 2021.

These directions assume you have a [fully configured](setup) history.state.gov Development Environment and that you need to update your system so that it contains the latest operating system, software dependencies, core applications, and configurations.

## Install system updates

1. From the Apple () menu in the top-left corner of your screen, choose `About This Mac`. Select the `Software Update` button and install any available software updates. If you are running a version of macOS lower than 11 (Big Sur), here is the link to the [macOS Big Sur webpage](https://support.apple.com/en-us/HT201475). 
    - If your computer cannot run macOS Big Sur (11), worry not; all of our software is still compatible with systems as old as macOS Mojave (10.14), but the directions below assume that you have Big Sur.

1. Open the App Store (using the Spotlight (🔍) icon in the menu bar, search for `App Store`; or in Finder, select `Go` > `Applications`, and find `App Store` in the list of applications), select the Updates tab, and install all available updates. 

## Update the Apple Developer Command Line Tools

1. After installing all system updates, you will need to install the current version of the Apple Developer Command Line Tools. To do so, open Terminal (using Spotlight, search for `Terminal`; or in Finder, select `Go` > `Utilities`), and paste in the following command:

        xcode-select --install

1. If this command returns a message that says, `error: command line tools already installed`, then you can skip to the next step. Otherwise, you will see a dialog window with 3 buttons. Select `Install`, and let the installation complete. Or, it might tell you that you already have these tools installed, in which case you should proceed.

## Update Homebrew

1. We need to update Homebrew. To do so, enter the following command: 

        brew update && brew upgrade

    This may take several minutes if it's been a long time since you've performed this action. Homebrew may display an error, saying that it cannot directly update a "shallow clone," in which case you should run the two additional `git` commands that it specifies. Then re-run the original command above to update Homebrew.

## Update HSG software dependencies

1. With Homebrew updated, we will use it to make sure you have all of the HSG software dependencies installed. Run the following commands to install any missing dependencies:

        brew tap bell-sw/liberica
        
        brew install liberica-jdk8-full exist-db github openvpn-connect
        
        brew reinstall oxygen-xml-editor

        brew install ant git maven node node@10

        npm install -g gulp bower
        
    If any of the applications are already installed, Homebrew will skip them, retaining the existing installation.

1. Make sure that the following icons are in your Dock: eXist-db, oXygen XML Editor, GitHub Desktop, and OpenVPN Connect. If any are not there, go to Finder, and select `Go > Applications`, and drag the respective applications' icons into the dock.

1. Run `brew doctor` to check your Homebrew installation, and follow any instructions to resolve problems that it reports. Keep running `brew doctor` until it reports:

    > Your system is ready to brew.

    Sometimes the problems reported by `brew doctor` are inscrutable. Contact Joe if you are unable to decipher these.
    
## Prepare oXygen and eXist

1. Next, open oXygen.
    - From the External Tools toolbar menu (i.e., the green, triangle-shaped icon), select `Fetch updates for all repositories`. 
    - Quit and restart oXygen.
    - Then, from the External Tools toolbar menu, run the `Wipe eXist database` command (confirm "yes" when asked)

1. Next, we need to force eXist to complete one full start up, in order to work around some quirks of eXist and macOS:
    - Click on eXist's dock icon.
    - A dialog box will open asking if you want to open eXist. Select `Open`. 
        - If `Open` is not a choice in this dialog, right-click (or hold down the control key and left-click) on the eXist icon, and select `Options` > `Show in Finder`. The Applications folder will open in Finder with the eXist application icon highlighted. Right-click on this icon and select `Open` from the contextual menu. Then close the Applications window.
    - eXist's dock icon will start bouncing, then stop.
    - Click on eXist's dock icon again.
    - A dialog box will open showing eXist's configuration properties. Select `Save`. When prompted to create the data directory or confirm the location of the data directory, select `OK`.
    - The eXist splash screen will appear as eXist completes its startup routine.
    - Once the eXist splash screen disappears, click on the eXist menu bar icon and select `Quit`.

1. Return to oXygen.
    - From the External Tools toolbar menu, under `= Setup and maintenance =`, select
        - `1. Clone all repositories & resources`
        - `2. Apply Mac settings to hsg-project`
        - `3. Apply hsg-project settings to eXist`

1. If you use eXist to preview website content, then proceed to [Start eXist](setup#start-exist) and then perform the steps under [Deploy all repositories to eXist](setup#deploy-all-repositories-to-exist). 

1. If you publish content to the website, then proceed to [Configure VPN](setup#configure-vpn) and delete previous 1861 entries in oXygen's Data Source pane and Transmit, creating new entries with the settings detailed in [Browse HSG's eXist in oXygen's Data Source Explorer](setup#browse-hsgs-exist-in-oxygens-data-source-explorer) and [Connect to HSG with Transmit](connect-to-hsg-with-transmit). You will need an OpenVPN profile and new passwords for HSG; contact Joe for these.

1. Now you're all up to date!
