# FlowReader desktop releases

This repository publishes the desktop builds of [FlowReader](https://flowreader.io/) for macOS,
Windows and Linux. It holds release binaries and this README only: no source code and no
workflows.

Download the latest version from the
[releases page](https://github.com/flowreader-app/flowreader/releases/latest), or install it with
your package manager below. The browser extensions are distributed through the Chrome Web Store and
Firefox Add-ons, not from here.

## macOS

macOS 10.15 or later. One universal build for Apple silicon and Intel.

- Download `FlowReader-macos-<version>-universal.dmg` from the release, open it and drag
  FlowReader to Applications. The app is signed and notarized by Apple.
- Or with [Homebrew](https://brew.sh/):

  ```sh
  brew install --cask flowreader-app/tap/flowreader
  ```

## Windows

x64.

- Download `FlowReader-windows-<version>-x64.msi` from the release and run it.
- Or with [Scoop](https://scoop.sh/):

  ```powershell
  scoop bucket add flowreader-app https://github.com/flowreader-app/scoop-bucket
  scoop install flowreader-app/flowreader
  ```

The installer is **not code-signed**: the project holds no Authenticode certificate. Windows
SmartScreen shows an "unrecognized app" warning the first time it runs; choose **More info**, then
**Run anyway**. Check the file against `SHA256SUMS.txt` (below) before you do.

## Linux

x86_64. Install from the FlowReader package repositories, which update with the rest of the
system:

- **Debian and Ubuntu (apt)**:

  ```sh
  sudo install -d -m 0755 /etc/apt/keyrings
  curl -fsSL https://flowreader.io/apt/flowreader-archive-keyring.asc | sudo gpg --dearmor -o /etc/apt/keyrings/flowreader.gpg
  echo "deb [arch=amd64 signed-by=/etc/apt/keyrings/flowreader.gpg] https://flowreader.io/apt stable main" | sudo tee /etc/apt/sources.list.d/flowreader.list
  sudo apt update && sudo apt install flow-reader
  ```

- **Fedora (dnf)**:

  ```sh
  sudo curl -fsSLo /etc/yum.repos.d/flowreader.repo https://flowreader.io/rpm/flowreader.repo
  sudo dnf install flow-reader
  ```

- **Snap**, on any distribution with snapd. The second command connects the desktop keyring, which
  snaps do not get automatically; without it, signing in fails:

  ```sh
  sudo snap install flowreader
  sudo snap connect flowreader:password-manager-service
  ```

Or download a file from the release; every release carries three formats:

- **AppImage**, on any distribution: download `FlowReader-linux-<version>-x86_64.AppImage`, make it
  executable (`chmod +x`) and run it. The AppImage updates itself.
- **Debian and Ubuntu**: download `FlowReader-linux-<version>-amd64.deb` and install it with
  `sudo apt install ./FlowReader-linux-<version>-amd64.deb`.
- **Fedora**: download `FlowReader-linux-<version>-x86_64.rpm` and
  install it with `sudo dnf install ./FlowReader-linux-<version>-x86_64.rpm`.

The `.deb` and the `.rpm` do not update themselves: install the next release the same way.

## Verify a download

Every release carries `SHA256SUMS.txt`, which lists the SHA-256 checksum of each file in it. With the
downloaded files in one directory:

```sh
# Linux
sha256sum -c SHA256SUMS.txt --ignore-missing

# macOS
shasum -a 256 -c SHA256SUMS.txt --ignore-missing
```

```powershell
# Windows: compare the output with the line for the MSI in SHA256SUMS.txt
Get-FileHash .\FlowReader-windows-<version>-x64.msi -Algorithm SHA256
```

The checksums are the ones recorded when the release was built; the mirror workflow refuses to
publish a file whose bytes differ from that record.

## Issues

Bug reports and questions about the desktop app are welcome in this repository's
[issues](https://github.com/flowreader-app/flowreader/issues). Please include the FlowReader version
(Settings, About), your operating system and how you installed it.

## How releases get here

Releases are published here by the FlowReader maintainers' release tooling. Every file is checked
against the manifest recorded when the release was built before it is uploaded. This repository has
no workflows and no source code, and its content is generated: changes made directly here are
overwritten by the next sync.
