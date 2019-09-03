# Removing All Versions of Java

If Homebrew raises an error when you try to upgrade Java, chances are that you have an old version of Java installed by an old version of Homebrew, and the current version of Homebrew has lost all ability to remove it. Or you have an old version of Java installed by an old version of the Mac operating system. To fix the situation, the best approach is to paste several commands into the Terminal which will definitively remove all traces of Java from your system.

```bash
sudo rm -rf /Library/Internet\ Plug-Ins/JavaAppletPlugin.plugin 
sudo rm -rf /Library/PreferencePanes/JavaControlPanel.prefPane 
sudo rm -rf ~/Library/Application\ Support/Oracle/Java
rm -r ~/"Library/Application Support/Oracle/Java"
sudo rm -rf /Library/Java/JavaVirtualMachines
```

For good measure, reset Homebrew:

```bash
brew update-reset && brew update
```

Now you should be able to install Java! Return to your directions—presumably you were somewhere in the [Setup](Setup) steps.

## Reference

For reference, see these links:

- https://www.java.com/en/download/help/mac_uninstall_java.xml
- https://www.java.com/en/download/help/deployment_cache.xml
- https://docs.oracle.com/javase/8/docs/technotes/guides/install/mac_jdk.html#A1096903
