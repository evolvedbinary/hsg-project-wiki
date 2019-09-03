# Removing All Versions of Java

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

## Reference

For reference, see these links:

- https://www.java.com/en/download/help/mac_uninstall_java.xml
- https://www.java.com/en/download/help/deployment_cache.xml
- https://docs.oracle.com/javase/8/docs/technotes/guides/install/mac_jdk.html#A1096903
