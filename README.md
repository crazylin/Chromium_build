# Chromium Build for Windows (x64)

This repository contains a GitHub Actions workflow to automatically build the Chromium browser for Windows x64 and publish it as a GitHub Release.

## Features
- **Automated Builds**: Automatically compiles Chromium on every push to `main`/`master`.
- **Manual Triggering**: Allows for manually starting a build with custom options.
- **Version Selection**: Ability to specify a specific Chromium version tag to build.
- **Optimized Release**: Builds an optimized Release version (`is_official_build=true`).
- **Codec Support**: Enables proprietary codecs like H.264 and AAC for full media support.
- **Build Caching**: Uses `sccache` to cache compiled objects, significantly speeding up subsequent builds to avoid the 6-hour GitHub Actions timeout.
- **Release Publishing**: Automatically packages the build and publishes it to the repository's Releases page.

## How to Use
1.  Navigate to the **Actions** tab of this repository.
2.  In the left sidebar, click on the **Build Chromium (Windows x64)** workflow.
3.  Click the **Run workflow** dropdown button.
4.  (Optional) Enter a specific Chromium version tag in the **Chromium version tag** field. You can find official tags on the [Chromium Dash Releases Page](https://chromiumdash.appspot.com/releases). If left empty, the latest version from the `main` branch will be used.
5.  Click the green **Run workflow** button.
6.  Wait for the workflow to complete. This can take several hours.
7.  Once finished, go to the **Releases** page of the repository to download the compiled `.zip` archive.

## Customization

### Modify Build Version
You can easily build a specific version of Chromium using the manual trigger, as described in the "How to Use" section.

### Modify Build Parameters
Chromium uses **GN (Generate Ninja)** to configure its build. All build arguments are defined in the `.github/workflows/build-chromium.yml` file within the `Generate build files (GN)` step.

You can modify the `--args` string to change the build configuration. For example:
```yaml
- name: Generate build files (GN)
  # ...
  run: |
    # ...
    gn gen out/Default --args='is_debug=true symbol_level=2' # Example: create a debug build
```
A full list of build arguments can be explored via `gn args --list out/Default` on a local machine with the Chromium checkout.

## Workflow Explained
The build process is defined in `.github/workflows/build-chromium.yml` and consists of the following main steps:
1.  **Trigger**: The workflow starts on a push or manual dispatch.
2.  **Setup Runner**: The job begins on a `windows-latest` runner.
3.  **Checkout Code**: Checks out the repository's code.
4.  **Free Up Disk Space**: Removes large, unnecessary software from the runner to make space for the Chromium source and build artifacts (over 100 GB).
5.  **Install depot_tools**: Clones Google's `depot_tools`, which are required for checking out and building Chromium.
6.  **Fetch Chromium Source**: Downloads the Chromium source code using `fetch`.
7.  **Checkout Specific Version**: If a version tag was provided, it checks out that specific version.
8.  **Sync Dependencies**: Runs `gclient sync` to download all necessary dependencies for the specified version.
9.  **Setup Caching**: Starts `sccache` to cache compilation results.
10. **Generate Build Files**: Runs `gn gen` with the specified arguments to create the build configuration.
11. **Compile Chromium**: Compiles the `chrome` target using `autoninja`, which leverages all available CPU cores. Cache statistics are shown after the build.
12. **Package & Release**: Compresses the build output and creates a GitHub Release with the artifacts.

---
_This repository and its workflow are maintained with the help of an AI pair programmer._ 