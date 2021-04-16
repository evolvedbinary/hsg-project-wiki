# Update your history.state.gov Development Environment

> Last updated on April 15, 2021.

These directions assume you have a [fully configured](setup) history.state.gov Development Environment and that you need to update your system so that it contains the latest operating system, software dependencies, core applications, and configurations.

## Install system updates

1. From the Apple () menu in the top-left corner of your screen, choose `About This Mac`. Select the `Software Update` button and install any available software updates. If you are running a version of macOS lower than 11 (Big Sur), here is the link to the [macOS Big Sur webpage](https://support.apple.com/en-us/HT201475) listing requirements and instructions for upgrading. 
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

1. Make sure that the following icons are in your Dock: eXist-db, oXygen XML Editor, GitHub Desktop, and OpenVPN Connect. If any are not there, go to Finder, and select `Go > Applications`, and drag the respective applications' icons into the dock. (It's likely that oXygen XML Editor will disappear from your Dock as a result of the command above, due to [a known issue with Homebrew](https://github.com/Homebrew/homebrew-cask/issues/102721). Please add oXygen back to your Dock for easy access.)

1. Run `brew doctor` to check your Homebrew installation, and follow any instructions to resolve problems that it reports. Keep running `brew doctor` until it reports:

    > Your system is ready to brew.

    Sometimes the problems reported by `brew doctor` are inscrutable. Contact Joe if you are unable to decipher these.
    
## Prepare oXygen and eXist

1. Next, open oXygen.
    - From the External Tools toolbar menu (i.e., the green, triangle-shaped icon), select `Pull updates for all repositories`. 
    - Quit and restart oXygen.

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

1. If you use eXist to preview website content, then start eXist via its application icon in your Dock. During startup eXist will show a splash screen. Once the splash screen disappears, you can start, stop, and quit eXist and access other eXist utilities via its menu bar entry (a blue "X"-shaped icon). 

1. To prepare your local eXist database with all of the files needed to run a local copy of HSG, go to the Tools dropdown menu in oXygen and select `4. Deploy all repositories to localhost`. This step takes about 10-15 minutes on our computers. On a remote computer, this step could take as long as 40 minutes, depending on your computer's hardware.

1. Now a complete copy of history.state.gov is now running at <http://localhost:8080/exist/apps/hsg-shell/>. 

## Set up OpenVPN Connect

Before you can resume publishing your work to HSG, you will need to set up the OpenVPN Connect application to connect to the new HSG Virtual Private Network (VPN). This allows you to access the production eXist server for publishing to HSG. 

Before starting this update, Joe should have provided you with an OpenVPN Profile file (it has a `.ovpn` file extension). If you don't have this file yet, contact Joe to request yours. To ensure this file is stored in a safe place, create a folder for it. In Finder, open your `workspace` folder, and use `File` > `New Folder` to create a new folder, called `OpenVPN Profiles`. Drag your OpenVPN Profile file (the `.ovpn` file) into this folder.

1. Start OpenVPN Connect via its application icon in your Dock.

1. Select the `X` icon to end the `Onboarding Tour`, select `Agree` when prompted with the Data Collection notice, and select `OK` to dismiss the "Updates" window.

1. On the `Import Profile` window, select the `File` tab.

1. Select the `Browse` button and navigate to your OpenVPN Profile file.

1. Select the `Add` button at the top-right corner of the window.

1. Toggle the connect/disconnect button to confirm you are able to connect. With the connection active, try opening <http://1861.hsg> in your browser. Now disconnect.

Now, anytime you need to publish to HSG, you can connect to the HSG VPN.

## Update oXygen and Transmit to publish via VPN

We previously published our work to HSG by connecting to the eXist server at "1861.history.state.gov." This domain is no longer accessible on the public internet. Instead, we now publish to HSG via the new "1861.hsg" domain, which is only accessible when connected to the VPN. To ensure you can use oXygen and Transmit to publish to HSG, perform these steps:

### Update oXygen to use 1861.hsg

1. In oXygen, select the `Window` menu > `Show View` > `Data Source Explorer`. A new pane will open up, called `Data Source Explorer`. 

1. In this pane's toolbar, click on the small gear icon (its tooltip labels this icon as `Configure Database Sources...`). A `Preferences` window will appear. 

1. Under `Connections`, find the entry `1861.history.state.gov`, and double-click on it to bring up the `Connection` window. 

1. Modify the `Name` and `WebDAV/FTP URL` fields as follows, leaving the other fields unchanged: 

    - `Name:` `1861.hsg`
    - `WebDAV/FTP URL:` `http://1861.hsg/exist/webdav/db`

1. Select `OK` to dismiss the `Connection` window, and select `OK` to dismiss the `Preferences` window.

1. In the `Data Source Explorer` pane, browse the contents of the `1861.hsg` entry and make sure you can reach the `apps` folder.

### Update Transmit to use 1861.hsg

1. In Transmit, select the "Servers" tab to see your list of saved servers.

1. Find the entry `1861.history.state.gov`, and right-click on it and select `Edit "1861.history.state.gov"` (or use the `Server` menu and select `Edit "1861.history.state.gov"`). 

1. Modify the server name, `Protocol`, `Address`, and `Port` fields as follows, leaving the other fields unchanged:

    - Name: `1861.hsg`
    - `Protocol`: `WebDAV` (i.e., not `WebDAV HTTPS`)
    - `Address`: `1861.hsg`
    - `Port`: `80`

### Update Transmit to use the new S3 buckets

1. If you publish to S3, contact Joe for the new S3 credentials.

Congratulations! Now you are all up to date!
