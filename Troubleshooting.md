# Troubleshooting

This article contains troubleshooting tips for problems that others have encountered. Hopefully this article will give you a path to finding a solution or workaround for any problem.

Have an idea for a new entry - a problem you've helped someone fix? Go ahead and add it!

## The directions say X should happen, but it's not

Please let your trainer or the author of the directions know where the directions are diverging from what you're experiencing so they can help find the problem you're encountering and/or fix the directions. The sooner we fix the problem, the better.

If you're getting an error, copy and paste the entire error verbatim into an email, or take a screenshot and send it. 

If the error is appearing on a web page, copy and paste the URL where the error appears into the email.

## Deploying all repositories to localhost says BUILD FAILED

First, check:

1. Are your copies of eXist and oXygen up-to-date with the versions stated in the [Setup](setup) document?
1. Have you run "Pull updates" in oXygen today? (Do this to make sure you have the latest files.) Does the console window that appears when you run "Pull updates" show any problems? If so, copy and paste the entire contents of the console into an email. If not, then try deploying the repositories to localhost again.
1. Is eXist running?

If the problem persists, copy and paste the entire contents of the console window in oXygen into an email and send it to your trainer. The console output can be cryptic, but it often indicates the source of the problem or provides useful clues.

## Uploading a file to localhost or hsg says BUILD FAILED

Causes can include:

1. For localhost, eXist is not running.
2. For history.state.gov, your connection to the internet is not working, or history.state.gov is undergoing maintenance, or you have not [entered the server credentials](https://github.com/HistoryAtState/hsg-project/wiki/setup#publish-your-work-to-hsg) for history.state.gov. 

The console window in oXygen should show a more detailed description of the problem. The console output can be cryptic, but it often indicates the source of the problem or provides useful clues. 

If you're not able to solve the problem, then copy and paste the entire contents of the console window in oXygen into an email and send it to your trainer.

## GitHub Desktop says there is a conflict

Git conflicts can be tricky to overcome, but take a screenshot and send it to your trainer. 

If all else fails, try to save your work to another folder. Then delete the repository from your disk and re-run the "Clone all repositories" step in the HSG Setup directions. This will put a fresh clone of the GitHub repository onto your disk. Then you can try to fold your work back into the repository so you can commit it.

## Remove all versions of Java

If Homebrew raises an error when you try to upgrade Java, chances are that you have an old version of Java installed by an old version of Homebrew, or you have an old version of Java installed by an old version of the Mac operating system—and Homebrew has no means to remove it. 

To fix the situation, open Terminal (using Spotlight, search for Terminal; or in Finder, select Go > Utilities), and paste in the following commands—which will cumulatively remove all traces of Java from your system:

```bash
sudo rm -rf /Library/Internet\ Plug-Ins/JavaAppletPlugin.plugin 

sudo rm -rf /Library/PreferencePanes/JavaControlPanel.prefPane 

sudo rm -rf ~/Library/Application\ Support/Oracle/Java

rm -r ~/"Library/Application Support/Oracle/Java"

sudo rm -rf /Library/Java/JavaVirtualMachines
```

Don't worry if any of the commands return a "No such file or directory" error. We're just trying to delete any that do exist.

For good measure, reset Homebrew:

```bash
brew update-reset && brew update
```

Now you should be able to install Java without error! Return to your directions—presumably you were somewhere in the [Setup](Setup) steps.

For reference, see these links:

- https://www.java.com/en/download/help/mac_uninstall_java.xml
- https://www.java.com/en/download/help/deployment_cache.xml
- https://docs.oracle.com/javase/8/docs/technotes/guides/install/mac_jdk.html#A1096903

## Transmit is refusing to connect to localhost!

You may need to downgrade to Transmit 5.2.3, since later versions have a bug that causes Transmit to fail when connecting to localhost; find version 5.2.3 at https://download.panic.com/transmit/. Also, confirm you've configured Transmit as described in [Connect to hsg with Transmit](setup#connect-to-hsg-with-transmit).