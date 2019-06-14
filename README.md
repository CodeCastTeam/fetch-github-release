# Download Github Release

A node module to download Github release assets. It will also uncompress zip
files and skip downloading if a file already exists.

[![Build Status](https://travis-ci.org/terascope/fetch-github-release.svg?branch=master)](https://travis-ci.org/terascope/fetch-github-release)
[![codecov](https://codecov.io/gh/terascope/fetch-github-release/branch/master/graph/badge.svg)](https://codecov.io/gh/terascope/fetch-github-release)
[![Build Status](https://david-dm.org/terascope/fetch-github-release.svg)](https://david-dm.org/terascope/fetch-github-release)

```
$ fetch-github-release -s darwin-x64 electron electron
Downloading electron/electron@v1.3.1...
electron-v1.3.1-darwi... ▇▇▇▇▇---------------------------------------------------- 662.8s
electron-v1.3.1-darwi... ▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇--------- 13.4s
electron-v1.3.1-darwi... ▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇--- 3.6s
ffmpeg-v1.3.1-darwin-... ▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇ 0.0s
```


## API

### Usage

If you need to download assets from a private repository or you need to avoid rate limits, you can pass an option token for the request. To generate a token go to your Github [settings](https://github.com/settings/tokens) and a token with `public_repo` or `repo` (for private repos) permissions.


```javascript
const downloadRelease = require('fetch-github-release');

const user = 'some user';
const repo = 'some repo';
const token = process.env.GITHUB_TOKEN
const outputdir = 'some output directory';
const leaveZipped = false;
const disableLogging = false;

// Define a function to filter releases.
function filterRelease(release) {
  // Filter out prereleases.
  return release.prerelease === false;
}

// Define a function to filter assets.
function filterAsset(asset) {
  // Select assets that contain the string 'windows'.
  return asset.name.indexOf('windows') >= 0;
}

downloadRelease(user, repo, token, outputdir, filterRelease, filterAsset, leaveZipped, disableLogging)
  .then(function() {
    console.log('All done!');
  })
  .catch(function(err) {
    console.error(err.message);
  });
```

`downloadRelease` returns an array of file paths to all of the files downloaded
if called with `leaveZipped = true`.
