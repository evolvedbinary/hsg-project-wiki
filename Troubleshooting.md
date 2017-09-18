# Troubleshooting

This article contains troubleshooting tips for problems that commonly crop up. In general, before you report a problem, make sure that your system is up-to-date with all system updates and with the updates at the top of the [Setup](setup) article.

## The directions say X should happen, but it's not

Please let your trainer or the author of the directions know where the directions are diverging from the directions so they can help find the problem you're encountering and/or fix the directions. 

If you're getting an error, copy and paste the entire error verbatim into an email, or take a screenshot and send it. 

If the error is appearing on a web page, copy and paste the URL where the error appears into the email.

## Uploading a file to localhost or history.state.gov says BUILD FAILED

The console window should show a more detailed description of the problem. The error messages can be cryptic, but they may point to the source of the problem.

At worst, copy and paste the entire contents of the console window in oXygen into an email and send it to your trainer.

## Deploying all repositories to localhost says BUILD FAILED

First, check:

1. Are your copies of eXist and oXygen up-to-date with the versions stated in the [Setup](setup) document?
1. Have you run "Fetch updates" in oXygen today? (Do this to make sure you have the latest files.) Does the console window that appears when you run "Fetch updates" show any problems? If so, copy and paste the entire contents of the console into an email. If not, then try deploying the repositories to localhost again.

If the problem persists, copy and paste the entire contents of the console window in oXygen into an email and send it to your trainer.

## GitHub Desktop is saying there is a conflict

Git conflicts can be tricky to overcome, but take a screenshot and send it to your trainer. 

If all else fails, try to save your work to another folder. Then delete the repository from your disk and re-run the "Clone all repositories" step in the HSG Setup directions. This will put a fresh clone of the GitHub repository onto your disk. Then you can try to fold your work back into the repository so you can commit it.