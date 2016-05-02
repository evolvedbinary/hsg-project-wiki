# Setting up a history.state.gov development system

Setting up a history.state.gov development system requires installing oXygen XML Editor, GitHub Desktop, checking out GitHub repositories, setting up oXygen to access the files in the history.state.gov project, starting eXist-db, and populating the database. Then you will have a complete copy of the history.state.gov website running on your computer, and you can edit files and preview how the changes will look on your computer. Once you have completed these steps, read the page on [version control](version-control) to learn how to save your work into our version control system and ensuring you continue to have the latest version of all files in our repository.

## Requirements

- Mac OS X (Not strictly necessary; on Linux or other operating systems, you should be able to find alternative methods of installing dependencies; but we assume Mac OS X here.)
- oXygen XML Editor (v17.1)
- Credentials for your GitHub account 

## Installing oXygen

1. Go to https://www.oxygenxml.com/xml_editor/software_archive_editor.html and download the edition of oXygen 17.1 called, "OS X 10.8 and later." Direct link: http://archives.oxygenxml.com/Oxygen/Editor/InstData17.1/MacOSX/VM/oxygen.tar.gz. Open the downloaded file, and drag the oXygen folder to your `Applications` folder (in Finder, select `Go` > `Applications`).

1. Start oXygen and paste in license key when prompted

## Installing other dependencies

1. Open Terminal (using Spotlight, search for `Terminal`; or in Finder, select `Go` > `Utilities`). Paste the following commands into your Terminal window, one at a time:

        mkdir ~/workspace

        open ~/workspace

    These commands created a new directory, `workspace`, in your home directory, and opened this directory in the Finder. We will be storing all files related to our work in this directory.

1. Install [Homebrew](http://brew.sh) and [Caskroom](http://caskroom.io/) by pasting the following commands into your Terminal window:

        /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"

        brew tap caskroom/cask

1. Having installed homebrew, install Java 8 JDK and GitHub Desktop, as well as the other dependencies:

        brew cask install java github-desktop

        brew install ant git node

        npm install -g gulp bower 

    When prompted, enter the password for your user account. You will now find GitHub Desktop in the `Applications` folder of your home directory (e.g., `/Users/Joe/Applications`, not in the top level `/Applications` directory). To find the application in Finder, select `Go > Home > Applications`.

1. Start GitHub Desktop, and follow these prompts to set up the program: Select `Continue`. Enter your GitHub user credentials, and select `Sign In`. Select `Continue`. Select `Install GitHub Command Line Tools`; when prompted, enter the password for your user account. Select `Done`. You are welcome to follow the tutorial to learn more about working with GitHub.

1. To clone the hsg-project repository which contains all of the files needed for working with history.state.gov, select the "+" icon in the top-left corner of the GitHub Desktop window. Select the Clone tab. Under HistoryAtState, you will see an entry called `hsg-project`. Select this, and then select `Clone hsg-project`. You will be prompted to select a location to save the `hsg-project` repository. Navigate to the `workspace` folder we created in step 1, and select `Clone`.

1. In Finder, navigate to your `workspace` directory, and notice that `hsg-project` is saved here. In `hsg-project`, double-click on the `hsg-project.xpr` file. This opens oXygen and opens the `Project` pane with a list of the top level folders in the `hsg-project` folder. You can use this to open any file in our project for editing.

1. In oXygen's menu bar, you will see a button with a green triangle icon, the "Tools" icon, and at its right is a small black triangle icon. Select the black triangle icon to display a list of common commands for working with the hsg-project. Select `Clone all repositories`. (These commands are also available via the oXygen menu bar: `Tools` > `External Tools`.) This triggers a command to clone all of the data repositories for hsg, including `frus` (for the _Foreign Relations_ series), `pocom` (for Principals & Chiefs), etc. These repositories will all be stored in the `repos` subdirectory of the `hsg-project` directory. On our DIN, `Clone all repositories` procedure should take only a couple of minutes; on slower internet connections, this may take 15 minutes or longer. You will know the command completed successfully when you see `BUILD SUCCESSFUL` in the console pane at the bottom of your oXygen window. 

1. In the Project pane, right-click on the `hsg-project` folder and select `Refresh`. You should now see a `repos` folder. Explore the `repos` directory and notice, for example, `frus` and `pocom`. These are all of the files that make up hsg.

1. Clone eXist to your `workspace` folder, `~/workspace/eXist-LTS`. [TBA]

1. In the Tools dropdown menu, select `Update and build eXist`; when this is complete, you will see `BUILD SUCCESSFUL`. In the Tools dropdown menu, select `Start eXist`; you will see a splash screen, which will go away when eXist has completed its startup. In the Tools menu, select `Deploy all repositories to localhost`.

[Note: currently hsg-shell fails unless you apply https://github.com/eXistSolutions/hsg-shell/commit/734c300746546901f8203ed519b36677724a396f.]

1. Now a complete copy of history.state.gov is now running at <http://localhost:8080/exist/apps/hsg-shell/>. 

## Editing files and previewing them in eXist-db

1. To open a file, use the Project window to drill down through the folder structure of the repositories. For example, drill into `repos/rdcr/countries/afghanistan.xml`. This is a TEI XML file that can be viewed at <http://localhost:8080/exist/apps/hsg-shell/articles/afghanistan> (see <http://history.state.gov/countries/afghanistan> for the public website).

1. Make a simple change, e.g., change the word "Summary" to "Introduction" in the first `<head>` element. Select the `File` menu > `Save` to save the file. 

1. To upload the file to eXist-db, select the `Tools` dropdown > `Upload current file to localhost`. A new tab will open at the bottom pane of the oXygen window, showing the results of the `Upload current file to localhost` script. When you see `BUILD SUCCESSFUL`, close the tab.

1. In your browser, (re-)load <http://localhost:8080/exist/apps/hsg-shell/countries/afghanistan>, and you will see your change. Return to oXygen, undo your change (`Edit` > `Undo`), save the file, and select `Upload current file to localhost` again.