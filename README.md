# Chromium Build for Windows (x64)

This repository contains a GitHub Actions workflow to automatically build the Chromium browser for Windows x64.

## How it works

The build process is defined in the `.github/workflows/build-chromium.yml` file. It automates the following steps:

1.  Sets up the build environment on a GitHub-hosted runner, including freeing up disk space.
2.  Checks out the Chromium source code using Google's `depot_tools`.
3.  Configures the build for a Release version with proprietary codecs (H.264/AAC) enabled.
4.  Compiles Chromium using `autoninja`.
5.  Packages the build output into a `.zip` archive.
6.  Creates a new GitHub Release and uploads the archive for public download.

## How to use

1.  **Trigger a build**: A new build is automatically triggered every time a commit is pushed to the `main` or `master` branch. You can also trigger a build manually from the repository's "Actions" tab.
2.  **Download the result**: Once the build is complete, go to the "Releases" section of this repository. The latest build will be available for download as a `.zip` archive.

---

_This repository and its workflow are maintained with the help of an AI pair programmer._ 