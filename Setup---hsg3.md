# Setting up a history.state.gov development system

Setting up a history.state.gov development system requires installing oXygen XML Editor, checking out GitHub repositories, setting up oXygen to access the files in the history.state.gov project, starting eXist-db, and populating the database. Then you will have a complete copy of the history.state.gov website running on your computer, and you can edit files and preview how the changes will look on your computer. Once you have completed these steps, read the page on [version control](version-control) to learn how to save your work into our version control system and ensuring you continue to have the latest version of all files in our repository.

## Requirements

- Mac OS X (Not strictly necessary; on Linux or other operating systems, you should be able to find alternative methods of installing dependencies; but we assume Mac OS X here.)
- oXygen XML Editor (v17.1)
- Credentials for your GitHub account 

## Install dependencies

1. Open Terminal (using Spotlight, search for `terminal`)
2. Install [Homebrew](http://brew.sh) and [Caskroom](http://caskroom.io/) by pasting the following commands into your Terminal window:

        /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"

        brew tap caskroom/cask

3. Having installed homebrew, install Java 8 JDK and GitHub Desktop:

        brew cask install java github-desktop

4. [TBD: install other dependencies]