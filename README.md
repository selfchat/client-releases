# selfchat downloads

Private messages with your friends, end-to-end encrypted, with no account,
no phone number and no server in the middle.

**[Download the latest version](https://github.com/selfchat/client-releases/releases/latest)**

| Your computer | Download |
| --- | --- |
| Windows 10 or 11 | `selfchat_x.y.z_x64-setup.exe` |
| Mac (Apple silicon or Intel) | `selfchat_x.y.z_universal.dmg` |
| Linux | `selfchat_x.y.z_amd64.AppImage`, or the `.deb` |

Once installed, selfchat updates itself. Every update is signed, and the app
checks the signature before installing it.

## First launch

These early builds aren't signed with an Apple or Microsoft certificate yet:

- **Windows:** if SmartScreen says it protected your PC, choose **More info**,
  then **Run anyway**.
- **Mac:** right-click the app, choose **Open**, then **Open** again. If macOS
  says the app is damaged, run `xattr -cr /Applications/selfchat.app` in
  Terminal once.

## Try it with a friend

1. Both of you install selfchat and pick a name.
2. One of you opens **Add a friend** and sends the other their link (or shows
   the QR code).
3. The other pastes it in. Your chat opens, encrypted end to end.

Messages go straight from one computer to the other. When one of you is
offline, messages wait and send as soon as you're both online.

## About this repository

This repository only holds the installers and the update feed
(`latest.json`). The source lives in private repositories and is built here
by GitHub Actions, so every download comes from a recorded build.
