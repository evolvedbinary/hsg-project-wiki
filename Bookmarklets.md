- "View on history.state.gov": takes the current page (presumably pointed at a history.state.gov development URL like localhost:8080) and load the same page on the live history.state.gov site

    `javascript:(function(){location.href='http://history.state.gov'+window.location.pathname+window.location.search;})();`

- "View on history.state.gov (new tab)": same as above, but opens the page in a new tab

    `javascript:(function(){window.open('http://history.state.gov'+window.location.pathname+window.location.search);})();`

- "View on localhost hsg dev": takes the current page (presumably pointed at the live history.state.gov site) and load the same page on your history.state.gov dev system on localhost:8080. If you want to be able to send the link to someone else on the network to preview, change `localhost` to your computer name on the network.

    `javascript:(function(){location.href='http://localhost:8080'+window.location.pathname+window.location.search;})();`

- "View on localhost hsg dev (new tab)": same as above, but opens the page in a new tab

    `javascript:(function(){window.open('http://localhost:8080'+window.location.pathname+window.location.search);})();`