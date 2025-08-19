+++
title = "Converting deb packages to a snap"
date = 2025-05-08
+++

As a general rule I'm trying to set up all my installed software on my system to be strictly contained - mostly so I can have a separate `.config` and `.local` directory etc, so the cleanup after removing an application is easier.

However, not everything has a snap.
For example [Lychee Slicer](https://lychee.mango3d.io/) provides linux options for `deb` and `AppImage`.
Neither of these options do container isolation, therefore I wanted to build a snap for this.
Given the origin of snaps being Ubuntu it makes sense that you should be able to convert a `deb` to a `snap`.
After a quick search I didn't find the obvious way of achieving this, only finding references to making use of the `staging.packages` options.
In this case, there isn't a repository to reference, but a downloadable `deb` file.

However, the `dump` plugin supports the deb as a source, making this straightforward.
You can just add the `deb` as a source and any dependencies to `stage-packages` and the rest should be straightforward, to reference the binary in the installed deb location and it should work:

```yml
name: lychee-slicer
base: core24
version: '7.3.2'
summary: Lychee Slicer
description: |
  Lychee Slicer provides a user-friendly interface and many powerful features that improve the slicing process.

grade: stable # must be 'stable' to release into candidate/stable channels
confinement: strict # use 'strict' once you have the right plugs and slots

parts:
  lychee-slicer:
    plugin: dump
    source: https://mango-lychee.nyc3.cdn.digitaloceanspaces.com/LycheeSlicer-7.3.2.deb
    stage-packages:
      - libgtk-3-0
      - libnotify4
      - libnss3
      - libxss1
      - libxtst6
      - xdg-utils
      - libatspi2.0-0
      - libuuid1
      - libsecret-1-0
      - libxshmfence1

apps:
  lychee-slicer:
    extensions: [gnome]
    command: opt/LycheeSlicer/lycheeslicer
    plugs:
      - home
      - network
      - network-bind
```

For any comments or feedback please leave a comment on the PR: [Converting deb packages to a snap](https://github.com/gameldar/the-second-drawer/pull/4)
